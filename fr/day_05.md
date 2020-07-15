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



Nous pouvons aussi déplacer des variables de l'inventaire vers le playbook. Par exemple, nous allons placer 
décidons que la variable ansible_user appartient au playbook et plus à l'inventaire (oui c'est très discutable, mais ça illustre mon propos !).
L'inventaire s'écrit :

et le playbook :
(ansible_user en global)
ou encore
(ansible_user dans tasks)


Le playbook se lance par la simple commande :


Si nous voulons enchaîner avec la commande show version, cette fois rien de plus simple, nous ajoutons une tâche :

## Niveau 2 : intercepter la sortie

Maintenant, prenons un vrai use-case : Nous recherchons une adresse MAC sur un ensemble de switchs et devons afficher l'équipement qui possède cette adresse sur un port en access.

Bravo à ceux qui pensent à écrire l'inventaire ou veulent mettre un show mac address-table dans un module cli_command : le métier rentre.

Les plus prudents commenceront pourtant à faire l'opération à la main pour s'assurer d'utiliser les bonnes commandes.

 
 Maintenant, on peut écrire l'inventaire par exemple de la façon suivante :


Ensuite, nous complétons le playbook avec la commande validée lors de notre essai.


Alors voyons voir le résultat :


Bon ça fonctionne, mais la mise en forme du résultat n'est pas encore là, car nous n'interprétons pas la sortie du switch. C'est le rôle de la commande register !
Elle s'utilise conjointement avec un module :


Maintenant nous avons affecté une variable qui sera globale au reste du playbook.
Elle peu s'utiliser avec le pseudo-module debug pour l'afficher, avec le module template pour la consommer ou avec les filtres when pour conditionner la suite du playbook.

Ici, nous affichons le retour de la commande en debug et retournons un success seulement pour l'équipement qui  possède cette adresse MAC.

## Les autres paramètres d'un playbook  

D'autres possibilités sont offertes par Ansible. Parmi elles : 
|||
|-|-|
|block|définit un bloc d'action, permet de conditionner une suite d'actions|
|tag|conditionne une action ou un bloc par un tag qui est passé en paramètre à ansible-playbook|
|gather_facts|défini en en-tête, il demande à Ansible de récupérer des renseignements sur le host.|


Voilà, vous êtes maintenant  initiés à la puissance d'Ansible et pouvez commencer à créer vos premiers automates. J'espère vous garder encore pour 2 journées, le temps de découvrir les templates.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczNTU1NzcwLDE5MzQzMzUyMDYsLTI2MD
A0MDUyMSwxNDc2ODA4MTU3LDEyMDg4NDEwNCwtMTg2NDQ5MDc2
LDc1MTE3NDY4MiwxNjUyNzMzMjMyLC05NjA4MzEzM119
-->