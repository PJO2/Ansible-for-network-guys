# Jour IV : Le module cli_command

Aujourd'hui: petite journée, puisque nous allons la passer à découvrir un seul module. Mais elle va nous servir à consolider tout ce que nous avons vu jusqu'ici. 


La documentation du module est disponible ici :
[https://docs.ansible.com/ansible/latest/modules/cli_command_module.html](https://docs.ansible.com/ansible/latest/modules/cli_command_module.html)

Nous regardons attentivement le tableau **Parameters** qui  liste les paramètres attendus par le module, à passer en ligne de commande via --args ou -a. 

Pour cli_command, le seul paramètre obligatoire est *command*, et précise la commande à exécuter.

Les arguments à passer à Ansible commencent à prendre forme :
 - la sélection des routeurs  
 - -i le fichier inventaire 
 - -m le nom du module : cli_command
 - -a  les paramètres du module: command='ma commande'

On n'oubliera pas le paramètre **connection** :
 - -c : network_cli (nécessaire au module cli_command)

Dans le fichier inventaire, nous plaçons les variables spécifiques à chaque routeur :


ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

Les paramètres :

Le show interfaces {{ wan }}
``````
<!--stackedit_data:
eyJoaXN0b3J5IjpbODM2NjQ2NTUxLC0xMzY0MjgyMTQ0LDc3Mj
c4OTIxOCwxMzcxMjQ4MTZdfQ==
-->