
# Les templates



## Tout est template
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


Bien sûr, cet exemple n'est pas un exemple de ce qu'il faut faire, mais ce playbook est parfaitement fonctionnel :



## le module template

Comme attendu le module template permet de croiser un template texte avec des variables. Nous l'utiliserons pour deux principaux usages  :

 - Créer un fichier de configuration adapté à chaque équipement (création de service)
 - Editer un rapport particularisé (sueprvision)

Ici nous allons configurer une adresse IP sur l'interface  LAN du routeur.

Bien sûr le template ne connaît pas l'interface et l'adresse IP.

Le template est :



Pour appeler le module template, il faut le fichier template et 


Et ça ne marche pas. La faute au paramètre connection. Le template doit être généré localement, pas sur le router (qui aura du mal à en faire quelquechose). 


C'est bon pour le template, mais il va falloir l'envoyer sur le routeur. Heureusement, il y un module copy qui va s'en charger ... si on l'exécute encore une fois en local....

Et voilà un moyen de changer facilement la configuration des interfaces des 2 routeurs qui composent mon site.




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzMjg1MDEzNCwtODcyMDEzMDgzLC0xMz
k4MzkxNDIsMTM5NDY0NTAyOCw0NDYzODAxMTFdfQ==
-->