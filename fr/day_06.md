
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
            command: show {{ iib }}
        register: "{{ register_var }}"
    
      - name: debug output
        debug:
           var: output


Bien sûr, on y gagne pas en lisibilité, mais ça fonctionne et 


## mais il y a un module template

Comme attendu le module template permet de croiser un template texte avec des variables.

Ici nous allons configurer une adresse IP sur l'interface  LAN du routeur.

Bien sûr le template ne connaît pas l'interface et l'adresse IP.

Le template est :



Pour appeler le module template, il faut le fichier template et 


Et ça ne marche pas. La faute au paramètre connection. Le template doit être généré localement, pas sur le router (qui aura du mal à en faire quelquechose). 


C'est bon pour le template, mais il va falloir l'envoyer sur le routeur. Heureusement, il y un module copy qui va s'en charger ... si on l'exécute encore une fois en local....

Et voilà un moyen de changer facilement la configuration des interfaces des 2 routeurs qui composent mon site.




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTgzOTE0MiwxMzk0NjQ1MDI4LDQ0Nj
M4MDExMV19
-->