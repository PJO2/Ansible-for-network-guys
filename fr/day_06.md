
# Les templates

Nous avons vu comment utiliser les templates et le langage de description Jinja2, regardons à présent comment les utiliser dans Ansible.


## Tout est Jinja
Dans les playbooks Ansible, toutes les données sont  implicitement  converties par l'interpréteur Jinja2.

Par exemple, notre playbook *show ip interface brief* peut être écrit de la façon suivante :

    ---
    # show ip interface brief
    - hosts: all
      gather_facts: no
      connection: network_cli
      vars:
          # playbook data
          iib: ip interface brief
          iib_filter: DHCP
          register_var: output
          name: show ip int brief en mode cryptique
          params: command
          # inventory data
          ansible_network_os: ios
          ansible_user: cisco
          ansible_ssh_pass: cisco
    
      tasks:
      - name: "{{ name }}"
        cli_command:
            command: show {{ iib }} | ex {{ iib_filter }}
        register: "{{ register_var }}"
    
      - name: debug output
        debug:
           var: output


Bien sûr,  je ne vous encourage pas à écrire des playbooks de ce type, néanmoins il est parfaitement fonctionnel :
![Playbook result](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/jinja2playbooks.png)
Et il faut garder en tête à cette possibilité très simple à mettre en œuvre pour passer des paramètres à une commande (recherche d'une adresse MAC, d'une route, ...) ou, comme nous allons le voir, s'assurer de l'unicité d'une ressource.


## le module template

Comme attendu le module [template](https://docs.ansible.com/ansible/latest/modules/template_module.html) permet de croiser un template texte avec des variables. Nous l'utiliserons pour deux principaux usages  :

 - Créer un fichier de configuration adapté à chaque équipement (création de service)
 - Fournir un rapport particularisé d'une commande opérationnelle (supervision)

En exemple, nous cherchons à configurer en masse (ie: sur nos 2 routeurs de lab !) une adresse IP sur l'interface  LAN du routeur.
L'adresse IP et le nom de l'interface sont des variables du template.

Le template s'écrit assez simplement :

    interface GigabitEthernet3
      ip address  {{ rtr.ip_lan }} {{ rtr.mask_lan }}
    end

Les données (structurées) sont lues directement dans l'inventaire :

    [mon_reseau]
    rtr231 ansible_host=10.0.0.231 rtr="{ 'ip_lan': '192.168.231.1', 'mask_lan': '255.255.255.0' }"
    rtr232 ansible_host=10.0.0.232 rtr="{ 'ip_lan': '192.168.232.1', 'mask_lan': '255.255.255.0' }"
    

Pour appeler le module *template*, il faut passer en paramètres le fichier template (*src*) et le fichier destination (*dest*). Comme plusieurs sessions seront lancées en parallèle, nous pensons suffixer le fichier destination par le nom du routeur, pour éviter qu'il ne soit écrasé, d'où le playbook :

    ---
    # Configure LAN interface
    - hosts: all
      gather_facts: no
      vars:
           ansible_user: cisco
           ansible_ssh_pass: cisco
           ansible_network_os: ios
      tasks:
      - name: call template
        template:
            src: create_itf.j2
            dest: create_itf.{{inventory_hostname}}.confg


Et ça ne marche pas. La faute au paramètre *connection* qui essaie, une fois de plus, de générer  le template sur le router et se plaint d'avoir un peu de mal à y trouver un interpréteur Jinja2. 

![Playbook result](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/jinja2playbooks2.png)
La correction consiste à forcer le paramètre *connection* à *local*.
Le playbook corrigé s'écrit donc :

    ---
    # Configure  LAN interface
    - hosts: all
      gather_facts: no
      connection: local
      vars:
           ansible_user: cisco
           ansible_ssh_pass: cisco
           ansible_network_os: ios
           
      tasks:
      - name: call template
        template:
            src: create_itf.j2
            dest: create_itf.{{ inventory_hostname }}.confg

Et cette fois c'est bon, et notre template est interprété comme attendu :
![Playbook result](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/jinja2playbooks3.png)

Du coup, nous savons générer des fichiers équipements par équipements à partir du template. Ces fichiers ont été créés sur le serveur Ansible, et il reste à les envoyer sur les routeur.
Il existe plusieurs modules qui peuvent s'en charger (raw, net_up, copy, ...), ici nous utilisons *cli_command* pour aller chercher  le fichier de configuration sur le serveur Ansible en TFTP.

Le playbook complet devient donc :

    ---
    # Configure  LAN interface
    - hosts: all
      gather_facts: no
      vars:
           ansible_user: cisco
           ansible_ssh_pass: cisco
           ansible_network_os: ios
           ansible_command_timeout: 60
    
      tasks:
      - name: call template
        connection: local
        template:
            src: create_itf.j2
            dest: create_itf.{{ inventory_hostname }}.confg
    
      - name: activate configuration on router
        connection: network_cli
        cli_command:
            command: copy tftp://10.0.0.110/cfgs/create_itf.{{ inventory_hostname }}.confg running-config
            prompt: Destination filename
            answer: ''


![Playbook result](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/jinja2playbooks5.png)

Et voilà comment changer facilement la configuration sur un parc de routeurs !

![Playbook result](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/jinja2playbooks4.png)

# Conclusion

Mon tour d'horizon est maintenant terminé et vous avez maintenant un aperçu du rôle d'Ansible dans la chaîne d'automatisation.  

Maintenant, c'est à vous d'adapter Ansible à vos cas d'usages. 
Et pour vous lancer, il est raisonnable de démarrer par des projets simples et nécessitant peu de données, puis passer à des tâches plus complexes, nécessitant toujours peu de données,  pour terminer par les projets les plus ambitieux, ceux dont la structure de données doit être étudiée consciencieusement avant même de développer l'outil. 

En bref, voir large, commencer petit ! 


J'ai essayé de construire cette mini-formation d'Ansible en introduisant les nouvelles notions progressivement dans l'ordre qui me semblait le plus naturel possible.

 - Une première commande fonctionnelle
 - La lecture des paramètres nécessaires à Ansible
 - La construction d'Ansible en modules
 - Séparation des paramètres entre inventaire et module
 - Les modules les plus utilisés
 - Les playbooks YAML
 - Les templates Jinja2


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc0NjIyMTI3LDI3MDY5ODY1NSwtMjIyMT
UzMzc4LDE1NjMzMzA0NSwtNTAyNjkyNDA0LC05OTk1NTMwNzIs
MTQwNjAxNDYzNSw3ODU0NTQ2NjAsODAwNzU2OTIyLDQ1MjA5OD
AyMSwtMTg2MTgzNDA4MSwtOTM2MjYyMDA4LDIxMDY0ODE4MCwt
MTcwMzUxNTEzOCwtODcyMDEzMDgzLC0xMzk4MzkxNDIsMTM5ND
Y0NTAyOCw0NDYzODAxMTFdfQ==
-->