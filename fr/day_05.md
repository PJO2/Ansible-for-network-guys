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


Si nous voulons enchaîner avec la commande show version, cette fois rien de plus simple, nous ajoutons une tâche :

## Niveau 2 : intercepter la sortie

Maintenant, prenons un vrai use-case : Nous recherchons une adresse MAC sur un ensemble de switchs et devons afficher l'équipement qui possède cette adresse sur un port en access.

Bravo à ceux qui pensent à écrire l'inventaire ou veulent mettre un show mac address-table dans un module cli_command : le métier rentre.

Les plus prudents commenceront pourtant à faire l'opération à la main pour s'assurer d'utiliser les bonnes commandes.

 
 Maintenant, on peut écrire l'inventaire par exemple de la façon suivante :


Ensuite, nous complétons le playbook avec la commande validée lors de notre essai.


Alors voyons voir le résultat :


Bon ça fonctionne, mais la mise en forme du r 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM5NjAwMjY4NywtMTU0MTQ2MzgwMiw5MT
EyNzYyNDJdfQ==
-->