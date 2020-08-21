
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
Et il faut garder en tête à cette possibilité très simple à mettre en œuvre pour passer des paramètres à une commande (recherche d'une adresse MAC, d'une route, ...).


## le module template

Comme attendu le module [template](https://docs.ansible.com/ansible/latest/modules/template_module.html) permet de croiser un template texte avec des variables. Nous l'utiliserons pour deux principaux usages  :

 - Créer un fichier de configuration adapté à chaque équipement (création de service)
 - Fournir un rapport particularisé d'une commande opérationnelle (supervision)

En exemple, nous cherchons à configurer en masse (ie: sur nos 4 routeurs de lab !) une adresse IP sur l'interface  LAN du routeur.
L'adresse IP et le nom de l'interface sont des variables du template.

Le template s'écrit assez simplement :

    interface GigabitEthernet3
      ip address  {{ rtr.ip_lan }} {{ rtr.mask_lan }}
    end



Pour appeler le module *template*, il faut le fichier template (*src*) et le fichier destination (*dest*), d'où le playbook :

    ---
    # Create LAN interface
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
            dest: create_itf.confg


Et ça ne marche pas. La faute au paramètre connection, car le template doit être généré localement, non sur le router (qui risque d'avoir du mal à trouver un interpréteur Jinja2). 


Du coup, on sait générer des fichiers équipements par équipements à partir du template, mais ces fichiers ont été créés sur le serveur Ansible, et il va falloir les envoyer un par un sur le routeur. Heureusement, il y un module copy qui va s'en charger ... à condition d'exécuter encore une fois ce module en local....


Et voilà un moyen de changer facilement la configuration des interfaces des routeurs qui composent mon site.



# Conclusion

J'ai essayé de construire cette mini-formation d'Ansible en introduisant les nouvelles notions une à une dans l'ordre qui me semblait le plus naturel possible, au  regard de nos métiers.

 - Une commande fonctionnelle
 - Lecture des paramètres
 - La construction d'Ansible en modules
 - Séparation des paramètres vers l'inventaire
 - Les modules les plus utilisés
 - Les playbooks
 - Les templates

N'hésitez pas à réagir si 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzg1NDU0NjYwLDgwMDc1NjkyMiw0NTIwOT
gwMjEsLTE4NjE4MzQwODEsLTkzNjI2MjAwOCwyMTA2NDgxODAs
LTE3MDM1MTUxMzgsLTg3MjAxMzA4MywtMTM5ODM5MTQyLDEzOT
Q2NDUwMjgsNDQ2MzgwMTExXX0=
-->