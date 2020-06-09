## Jour V: Les playbooks

Nous avons vu comment lancer une commande, comment organiser les données. Continuons notre voyage avec l'organisation des actions.

Et oui, derrière ansible, il y a ansible-playbook qui permet d'exécuter une succession d'actions, appartenant à différents modules.

Et si je vous demande sous quelle forme va être écrit le playbook ? Eh oui : c'est du YAML, d'où le détour par lequel je vous ai fait passé la semaine dernière. 

Commençons par réécrire notre show clock en playbook.

D'abord, l'en-tête du playbook décrit les paramètres globaux. Ensuite, le champ tasks décrit la successions d'actions. 

Chaque action comporte au moins les champs module, et je vous conseille d'ajouter le champ name. Ce dernier est optionnel, mais il augmente la lisibilité du playbook et apparaît également dans l'exécution du playbook.

Bref, notre playbook devient :


Nous pouvons aussi déplacer des variables de l'inventaire vers le playbook. Par exemple, nous décidons que la varaible ansible_user appartient au playbook et plus à l'inventaire (oui c'est très discutable, mais ça illustre mon propos !).
L'inventaire s'écrit :

et le playbook :
(ansible_user en global)
ou encore
(ansible_user dans tasks)


Le playbook se lance par la simple commande :



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMDY4NjAzNzYsOTExMjc2MjQyXX0=
-->