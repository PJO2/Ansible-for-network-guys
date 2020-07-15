## Jour V: Les playbooks

Nous avons vu comment lancer une commande, comment organiser les données. Continuons notre voyage avec l'organisation des actions.

Et oui, derrière ansible, il y a ansible-playbook qui permet d'exécuter une succession d'actions, appartenant à différents modules.

Et si je vous demande sous quelle forme va être écrit le playbook ? Gagné : c'est du YAML, d'où le détour par lequel je vous ai fait passé la semaine dernière. 

Commençons par réécrire notre show clock en playbook.

D'abord, l'en-tête du playbook décrit les paramètres globaux, a minima la variable *hosts* qui filtre les hosts de l'inventaire . Ensuite, le champ tasks décrit la successions d'actions. 

Chaque action comporte au moins un champ qui est le nom du module à exécuter et je ne saurai trop vous conseiller d'y ajouter le champ optionnel *name*, afin d'augmenter la lisibilité du playbook et de la sortie, car ce champ apparaitra également dans l'exécution du playbook.

Bref, notre playbook devient :

    ---
    - hosts: all
      tasks:
        - name: un show clock en mode raw
          raw: show clock

Petite déception, une fois le playbook lancé par la commande

    ansible-playbook -i inv raw.yml

Il semble que la tâche *gathering facts* se soit invitée dans notre playbook pour le faire planter ! 

![playbook001](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook001.png) 
Et c'est exactement ça : pensant nous "faciliter la vie", Ansible récolte implicitement des informations sur les hosts, comme le type d'Unix installé, le type de processeur, la mémoire libre, l'espace disque, la version de python installée... 
Bref, un ami qu'on va débrancher au plus vite en ajoutant "gather_facts: no" en début de playbook :
    
    ---
    - hosts: all
      gather_facts: no
      tasks:
        - name: un show clock en mode raw
          raw: show clock

Et cette fois c'est bon :
![playbook002](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook002.png)
Enfin, c'est bon, si l'on se contente du OK ! 
Mais c'est le principe du playbook : lancer plusieurs actions consécutives et, par défaut, ne pas conserver le résultat.

Effectuons la même action avec le module cli_command. Le playbook devrait ressembler à quelque chose du type :

    ---
    # show clock
    - hosts: all
      gather_facts: no
      tasks:
      - name: un show clock via le module cli_command
        cli_command:
            command: show clock

L'erreur suivante devrait nous faire rapidement penser au fameux paramètre *connection*.

![playbook003](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook003.png)
Et de fait, si on le précise dans la commande :

    ansible-playbook -i inv show_clock.yaml -c network_cli
c'est beaucoup mieux
![playbook004](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook004.png)
Encore mieux, nous définissons ce paramètre dans le playbook qui devient :

    ---
    # show clock
    - hosts: all
      gather_facts: no
      tasks:
      - name: un show clock via le module cli_command
        cli_command:
            command: show clock

Nous pouvons aussi déplacer des variables de l'inventaire vers le playbook. Par exemple, nous 
décidons que les variables *ansible_network_os*, *ansible_user* et *ansible_ssh_pass* appartiennent au playbook et plus à l'inventaire 
L'inventaire s'écrit :

    [mon_reseau]
    mon_routeur ansible_host=10.0.0.230

et le playbook :

    ---
    # show clock
    - hosts: all
      gather_facts: no
      connection: network_cli
      vars:
         ansible_user: cisco
         ansible_ssh_pass: cisco
         ansible_network_os: ios
    
      tasks:
      - name: un show clock via le module cli_command
        cli_command:
            command: show clock

La chapitre *vars* pouvant être déclaré pour l'ensemble du playbook, comme nous l'avons fait, ou au niveau d'une tâche.


## Niveau 2 : intercepter la sortie

Maintenant, prenons un use-case plus opérationnel : Nous recherchons l'interface correspondant à une adresse IP donnée sur un ensemble de routeurs et devons afficher l'équipement qui possède cette adresse.

Vous avez déjà pensé à écrire l'inventaire par exemple de la façon suivante :

    [mon_reseau]
    rtr230 ansible_host=10.0.0.230
    rtr231 ansible_host=10.0.0.231
    rtr232 ansible_host=10.0.0.232
    rtr233 ansible_host=10.0.0.233

Ensuite, nous complétons le playbook avec la commande *show ip interface brief*.

    ---
    # show clock
    - hosts: all
      gather_facts: no
      connection: network_cli
      vars:
           ansible_user: cisco
           ansible_ssh_pass: cisco
           ansible_network_os: ios
    
    
      tasks:
      - name: lire les adresses du routeur
        cli_command:
            command: show ip interface brief


Alors voyons voir le résultat :
![playbook005](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook005.png)
Bon ça fonctionne, mais la mise en forme du résultat n'est pas encore là, car nous n'interprétons pas la sortie de la commande. C'est le rôle de la commande register !
Elle s'utilise conjointement avec un module :

    tasks:
      - name: execute show ip interface brief
        cli_command:
            command: show ip interface brief
        register: output


Une fois la tâche passée, la variable output sera globale au reste du playbook.
Elle peut s'utiliser avec le pseudo-module *debug* pour l'afficher, avec le module *template* pour la consommer ou avec les filtres *when* pour conditionner la suite du playbook.

Par exemple, nous lançons dans le playbook une seconde commande qui affiche le contenu de la variable :

      tasks:
      - name: execute show ip interface brief
        cli_command:
            command: show ip interface brief
        register: output
    
      - name: debug output
        debug:
           var: output

Voici la sortie pour le dernier routeur :
![playbook006](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook006.png)
La sortie du routeur est disponible sur plusieurs formes (*output.stdout*  ou *output.stdout_lines*).

Ici, nous ajoutons une tâche pour stopper l'exécution si le routeur ne gère pas une adresse donnée :
    
    ---
    - hosts: all
      gather_facts: no
      connection: network_cli
      vars:
           ansible_user: cisco
           ansible_ssh_pass: cisco
           ansible_network_os: ios
    
      tasks:
      - name: execute show ip interface brief
        cli_command:
            command: show ip interface brief
        register: output
    
      - fail:
        when: not '10.0.0.232' in output.stdout

![playbook007](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook007.png)
Enfin, notons que l'adresse recherchée peut être passée en paramètre au playbook via l'option --extra-arg ou -e. Elle est correctement interprétée dans le playbook :

      - fail:
        when: not ip in output.stdout

![playbook007](https://raw.githubusercontent.com/PJO2/Ansible-for-network-guys/master/images/playbook007.png)

## Les autres paramètres d'un playbook  

D'autres possibilités sont offertes par Ansible. Parmi elles : 
|||
|-|-|
|block|définit un bloc d'action, permet de conditionner une suite d'actions|
|tag|conditionne une action ou un bloc par un tag qui est passé en paramètre à ansible-playbook|
|gather_facts|défini en en-tête, il demande à Ansible de récupérer des renseignements sur le host.|


Voilà, vous êtes maintenant  initiés à la puissance d'Ansible et pouvez commencer à créer vos premiers automates. J'espère vous garder encore pour 2 journées, le temps de découvrir les templates.




<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxMzYzMTUxMywxNTE1MDM0OTYyLDI1MT
U1Njc5NCw4NDM5OTM3NjAsLTM0MTM0MjIxNSwxMzE3NzU5ODEw
LDE5MzQzMzUyMDYsLTI2MDA0MDUyMSwxNDc2ODA4MTU3LDEyMD
g4NDEwNCwtMTg2NDQ5MDc2LDc1MTE3NDY4MiwxNjUyNzMzMjMy
LC05NjA4MzEzM119
-->