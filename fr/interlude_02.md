
# Interlude  : les templates Jinja2


Un template ou Gabarit est un modèle qui permet de structurer un objet (pour nos un fichier texte) sans que les variables soient présentes.

Jinja2 est un langage d'écriture de  templates intégré à python et développé pour des écrires des pages HTML  ! Mais comme il est puissant et générique, nous allons l'utiliser pour écrire des configurations de routeurs...

En Jinja2, les éléments placés entre une double accolade sont interprétés. Par exemple le petit template suivant 

Hello {{name}}

rendra 

Hello Aline

si name vaut Aline. 
  
 Un peu plus compliqué, Jinja2 consomme des variables structurées de type Json ou Yaml, en les combinant par un point.

Si 

alors

Hello {{ jedis["

Un lien vers mon testeur de templates

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzkzNDcyMDU1LDQ5NzI4MDMzNSw3MzA5OT
gxMTZdfQ==
-->