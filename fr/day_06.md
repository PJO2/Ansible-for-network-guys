
# Les templates

## Tout est template
Dans les playbooks Ansible, toutes les données sont  implicitement  converties par lJinja2.

Par exemple, notre playbook show ip interface brief peut être écrit de la façon suivante :

 

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
eyJoaXN0b3J5IjpbLTE0OTA2MzM3NjIsMTM5NDY0NTAyOCw0ND
YzODAxMTFdfQ==
-->