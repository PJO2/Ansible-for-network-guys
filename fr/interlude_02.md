
# Interlude  : les templates Jinja2


## Définition
Un template ou Gabarit est un modèle qui permet de structurer un objet (pour nous un fichier texte) avec des éléments statiques et des éléments dynamiques non connus (variables). 

Jinja2 est un langage d'écriture de  templates intégré à python et développé pour des écrires des pages HTML  ! Mais comme il est puissant et générique, nous allons l'utiliser pour écrire des configurations de routeurs...

## Les variables
En Jinja2, les éléments placés entre une double accolade correspondant à des éléments dynamiques. Ils sont interprétés lors de la résolution du template. Par exemple le petit template suivant :

    Hello {{ name }}

rendra 

    Hello Aline

si name vaut Aline. 
  
 Un peu plus compliqué, Jinja2 consomme des variables structurées de type Json ou Yaml, en les combinant par un point.

Si jedis["obiwan"]["master"]  vaut qui-gon
alors

    Hello {{ jedis.obiwan.master }}

donnera :

    Hello qui-gon

Remplacer le point . par des crochets []  permet d'interpréter ce qu'il y a à l'intérieur des crochets :
Si jedi vaut obiwan, alors

    Hello {{ jedis[jedi].master }} 
 donnera :
 
    Hello qui-gon


## Les filtres
La notion suivante de Jinja2 est le filtre. Une variable suivie du signe | est envoyée vers le filtre indiqué.

Le filtre peut préciser une mise en forme ou un calcul. (Alors oui le terme filtre est sans doute mal choisi, mais c'est le terme utilisé par Jinja2 !).

Ainsi, si on souhaite saluer les padawans de obiwan, on évitera d'utiliser

    Hello {{ jedis.obiwan.padawans }}

 qui donne : 
 

    Hello ['anakin', 'luke']

 
 on écrira plutôt :
>  Hello {{ jedis.obiwan.padawans | join(',') }}

Les filtres peuvent être étendus en écrivant des procédures python. Ansible nous offre le filtre ipaddr qui  permet de faire des opérations sur les adresses IP.

## Les structures de contrôle

Jinja2 possède un mécanisme de test et un mécanisme de boucle. Ces instructions sont placés entre les signes {% et  %}.

Par exemple, nous pouvons parcourir notre variable  jedis avec l'instruction :

> {% for jedi in jedis %} 
> {% endfor %}`



 

Un lien vers mon testeur de templates

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MTY1ODY3OTYsLTE3OTA0MjQ1MzIsLT
Y2MTk5MDMyNiwxNjAwMjU1MDQ0LDIxMjkyMzg1NzcsNDk3Mjgw
MzM1LDczMDk5ODExNl19
-->