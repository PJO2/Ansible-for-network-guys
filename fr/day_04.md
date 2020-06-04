# Jour IV : Le module cli_command

Petite journée, puisque nous allons la passer à découvrir un seul module, mais  

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

ark@amp-ansible:~$ ansible all -i inv -m cli_command -a "command='show clock'" -c network_cli -e "ansible_network_os=ios"

Les paramètres :

Le show interfaces {{ wan }}
``````
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzQ1NzA3MjMsMTM3MTI0ODE2XX0=
-->