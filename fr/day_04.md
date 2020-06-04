# Jour IV : Le module cli_command

Aujourd'hui: petite journée, puisque nous allons la passer à découvrir un seul module. Mais elle va nous servir à manipuler simultanément actions et données. 

La documentation du module est disponible ici :
[https://docs.ansible.com/ansible/latest/modules/cli_command_module.html](https://docs.ansible.com/ansible/latest/modules/cli_command_module.html)

Nous regardons attentivement le tableau **Parameters** qui  liste les paramètres attendus par le module, à passer en ligne de commande via --args ou -a. 
Pour cli_command, le seul paramètre obligatoire est command, et précise la commande à exécuter.

Notre syntaxe se précise donc : 
 - la sélection des routeurs  
 - -i le fichier inventaire 
 - -m le nom du module : cli_command
 - -a  les paramètres du module: command='ma commande'

On n'oubliera pas le paramètre **connection** :
 - -c : network_cli (nécessaire 

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

Les paramètres :

Le show interfaces {{ wan }}
``````
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjA1NTI1OTU0LDc3Mjc4OTIxOCwxMzcxMj
Q4MTZdfQ==
-->