# Jour III :  actions et données :

Comme tout automate digne de ce nom, Ansible cloisonne les actions et les données. Les actions sont confiées aux modules, les données sont récupérées dans l'inventaire.

## Les actions : le module et les paramètres du module
Le module est l'entité d'Ansible qui effectue une tâche spécialisée. Nous avons vu qu'il y a des milliers de modules disponibles et nous allons en étudier 3.

La plupart des modules effectue les actions sur le serveur distant. Nous avons vu le module *raw* qui se contente de lancer la commande donnée en paramètre (ici *show clock*), ce qui est intuitif. Beaucoup moins intuitif pour nous, le module *copy* (qui copie des fichiers) ou le module *template* (qui sera vu en dernière journée) sont exécutés par défaut sur le serveur distant. Et ce n'est pas ce nous voulons en réseau. Je radote encore, mais Ansible a été écrit pour administrer des serveurs, pas des routeurs. Bref, c'est nous, administrateurs réseaux, qui avons dévoyé l'usage d'Ansible.
D'ailleurs, il y a même un module nommé *local* pour effectuer des actions sur la station Ansible.

La clef, c'est le paramètre --connection (ou -c) qui spécifie la portée du module. Par défaut, connection est positionné à ssh, mais nous pouvons aussi préciser *local*, pour que l'action soit faite localement par la station Ansible. Nous pouvons finalement utiliser des templates, copier des fichiers, sans demander à nos routeurs des fonctionnalités Unix.

En version 2.7, une troisième option est ajoutée : c'est la valeur cli_connection qui permet de contrôler un distant, via des commandes locales, à la manière du module raw.

## Les données

Jusqu'ici, nous avons pu contrôler un seul équipement distant, ce qui reste éloigné de notre objectif d'automatiser un parc.

Le parc d'équipement est décrit dans le fichier inventaire. Par défaut, Ansible le recherche à l'emplacement /etc/ansible/hosts, mais même en lab, je vous invite à utiliser un fichier différent, qui sera communiqué à Ansible par le paramètre --inventory (ou -i).

Ce fichier respecte la syntaxe des fichiers Ini, en étant organisé en sections.
Les sections représentent des groupes de serveurs distants. Les clefs sont les serveurs, les valeurs sont les variables spécifiques à un serveur. Enfin, sans surprise, la section pré-définie [all] est commune à l'ensemble des serveurs.
Nous pouvons par exemple organiser


Fichier ini

Variables de groupe, variables par host

group\_vars/

hosts\_vars

Créer une deuxième host et lancer la même action sur les 2 équipements.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjA2MjAzODQsLTIwNDYzNjUzODgsMT
k2MDE0NDQxMF19
-->