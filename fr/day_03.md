# Jour III :  actions et données :

Comme tout automate digne de ce nom, Ansible cloisonne les actions et les données. Les actions sont confiées aux modules, les données sont récupérées dans l'inventaire.

## Les actions : le module et les paramètres du module
Le module est l'entité d'Ansible qui effectue une tâche spécialisée. Nous avons vu qu'il y a des milliers de modules disponibles et nous allons en étudier 2.

La plupart des modules effectue les actions sur le serveur distant. Nous avons vu le module *raw* qui se contente de lancer la commande donnée en paramètre (ici *show clock*), ce qui est intuitif. En revanche, le module *copy* (qui copie des fichiers) ou le module *template* (qui sera vu en dernière journée) sont exécutés par défaut sur le serveur distant. Et ce n'est pas ce nous voulons en réseau. Je radote encore, mais Ansible a été écrit pour administrer des serveurs, pas des routeurs. Bref, c'est nous, administrateurs réseaux, qui avons dévoyé l'usage d'Ansible.



## Les données

Phase II Organiser les données dans l&#39;inventaire

Fichier ini

Variables de groupe, variables par host

group\_vars/

hosts\_vars

Créer une deuxième host et lancer la même action sur les 2 équipements.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODAzMDI1MTQsMTk2MDE0NDQxMF19
-->