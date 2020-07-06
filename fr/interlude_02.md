
# Interlude  : les templates Jinja2


Un template ou Gabarit est un modèle qui permet de structurer un objet (pour nous un fichier texte) sans que les variables soient définies.

Jinja2 est un langage d'écriture de  templates intégré à python et développé pour des écrires des pages HTML  ! Mais comme il est puissant et générique, nous allons l'utiliser pour écrire des configurations de routeurs...

En Jinja2, les éléments placés entre une double accolade sont interprétés. Par exemple le petit template suivant 

Hello {{name}}

rendra 

Hello Aline

si name vaut Aline. 
  
 Un peu plus compliqué, Jinja2 consomme des variables structurées de type Json ou Yaml, en les combinant par un point.

Si 
jedis["obiwan"]["master"]  vaut yoda
alors
Hello {{ jedis.obiwan.master }}
donnera :


La notion suivante de Jinja2 est le filtre. Une variable suivie du signe | est envoyée vers le filtre indiqué.

Le filtre peut préciser une mise en forme ou un calcul. (Alors oui le terme filtre est peut-être mal choisi, mais c'est le terme utilisé par Jinja2 !).

Ainsi, si on souhaite saluer les padawans de obiwan, on évitera d'utiliser
Hello {{ jedis.obiwan.padawans }}
 
 on écrira plutôt :
 Hello {{ jedis.obiwan.padawans | join(',') }}

Et n'oubliez pas de transmettre mes amitiés à Darth Vador !


Les filtres peuvent être étendus en écrivant des procédures python. Ansible nous offre le filtre netfilter? qui  permet de faire des opérations sur les adresses IP.


Les instructions {% %}



 

Un lien vers mon testeur de templates

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYwMDI1NTA0NCwyMTI5MjM4NTc3LDQ5Nz
I4MDMzNSw3MzA5OTgxMTZdfQ==
-->