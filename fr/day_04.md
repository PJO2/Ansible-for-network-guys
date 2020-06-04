# Jour IV : Le module cli_command

Aujourd'hui: petite journée, puisque nous allons la passer à découvrir un seul module. Mais elle va nous servir à consolider tout ce que nous avons vu jusqu'ici. 


La documentation du module est disponible ici :
[https://docs.ansible.com/ansible/latest/modules/cli_command_module.html](https://docs.ansible.com/ansible/latest/modules/cli_command_module.html)

Nous regardons attentivement le tableau **Parameters** qui  liste les données attendues par le module, qu'on passera via le paramètre --args ou -a. 

Pour cli_command, le seul paramètre obligatoire est *command*, et précise la commande à exécuter.

Les arguments à passer à Ansible commencent à prendre forme :
|||
|-|-|
||la sélection des routeurs|
|-i| le fichier inventaire |
|-m| le nom du module : cli_command|
|-a|  les paramètres du module: command='ma commande'|

On n'oubliera pas le paramètre **connection** :
|||
|-|-|
|-c|network_cli (nécessaire au module cli_command)|

Dans le fichier inventaire, nous plaçons les variables spécifiques à chaque routeur : 

 - les identifiants de connexion qui sont communs à notre parc
 - la résolution nom vers adresse IP, puisque mes équipements ne sont pas connus par le DNS 

Et je lance ma commande :
![screenshot006](screenshot006.png)

Caramba encore raté !

En revanche, nous avons un message d'erreur tout à fait explicite (ce qui n'est pas toujours le cas avec Ansible !) : il manque la variable *ansible_network_os*.  

Première question : où va-t-on renseigner cette variable. Vous avez en tête les différentes possibilités :

 - en global dans le fichier inventaire
 - pour chaque host du fichier inventaire
 - en ligne de commande avec le paramètre --extra-vars 

Ici je vais choisir le paramètre extra-vars pour mettre cette variable en avant, mais les autres choix sont respectables.

Le problème, c'est surtout ce qu'on va mettre dans cette variable et la documentation n'aide pas franchement. Les différents OS supportés se retrouvent dans les plugins python installés par Ansible. 



 
ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

Les paramètres :

Le show interfaces {{ wan }}
``````
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzY4NjY3NTMsLTY5OTQ2MTA2NiwxNDk2Nj
A2ODc5LC0xMzY0MjgyMTQ0LDc3Mjc4OTIxOCwxMzcxMjQ4MTZd
fQ==
-->