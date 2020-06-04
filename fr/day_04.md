# Jour IV : Le module cli_command

Aujourd'hui: petite journée, puisque nous allons la passer à découvrir un seul module. Mais elle va nous servir à manipuler simultanément actions et données. 

La documentation du module est disponible ici :
[https://docs.ansible.com/ansible/latest/modules/cli_command_module.html](https://docs.ansible.com/ansible/latest/modules/cli_command_module.html)

Nous regardons attentivement le tableau **Parameters** qui  liste les paramètres attendus par le module, à passer par le 

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

Les paramètres :

Le show interfaces {{ wan }}
``````
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjI0Njk0MzMzLDc3Mjc4OTIxOCwxMzcxMj
Q4MTZdfQ==
-->