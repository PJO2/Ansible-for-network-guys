
# Interlude  : les templates Jinja2


## Définition
Un template ou Gabarit est un modèle qui permet de structurer un objet (pour nous un fichier texte) avec des éléments statiques et des éléments dynamiques non connus (variables). 

Jinja2 est un langage d'écriture de  templates intégré à python et développé pour des écrires des pages HTML  ! Mais comme il est puissant et générique, nous allons l'utiliser pour décrire des configurations de routeurs...

## Les variables

En Jinja2, les éléments placés entre une double accolade correspondant à des éléments dynamiques. Ils sont interprétés lors de la résolution du template.  
Par exemple le petit template suivant :

    Hello {{ name }}

rendra 

    Hello Aline

si name vaut Aline. 
  
 Un peu plus compliqué, Jinja2 consomme des variables structurées en les combinant par un point.

Si jedis["obiwan"]["master"] vaut qui-gon (en prenant la syntaxe python), alors

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

Le filtre peut préciser une sélection, une mise en forme ou un calcul. (Oui, le terme filtre est sans doute mal choisi, mais c'est le terme choisi par Jinja2 !).

Ainsi, si on souhaite saluer les padawans de obiwan, on évitera d'utiliser

    Hello {{ jedis.obiwan.padawans }}

 qui donne : 
 

    Hello ['anakin', 'luke']

 
 on écrira plutôt :

    Hello {{ jedis.obiwan.padawans | join(', ') }}
pour avoir 

    Hello anakin, luke

Les filtres peuvent être étendus en écrivant des procédures python. Ansible nous offre le filtre ipaddr qui  permet de faire des opérations sur les adresses IP.

## Les structures de contrôle

Jinja2 possède un mécanisme de test et un mécanisme de boucle. Ces instructions sont placés entre les signes {% et  %}.

Par exemple, nous pouvons parcourir notre variable  jedis avec l'instruction :

     {% for jedi in jedis %} 
     {% endfor %}

Si nous utilisons notre base de données *jedis* :

    ---
    jedis:
        obiwan:
            location: tatooine
        yoda:
            location: dagobah

Et que nous voulons créer la table HTML suivante :
|name|location|
|-|-|
|obiwan|tatooine|
|yoda|dagobah|

Nous pouvons utiliser le template Jinja2 :

    <table>
       <tr><th>name</th><th>location</th></tr>
    {% for jedi in jedis %}
       <tr><td>{{ jedi }}</td><td>{{ jedis[jedi].location }}</td></tr>
    {% endfor %}
    </table>
    
qui donne :

    <table>
    <tr><th>name</th><th>location</th></tr>

    <tr><td>obiwan</td><td>tatooine</td></tr>

    <tr><td>yoda</td><td>dagobah</td></tr>
    
    </table>

Pas mal du tout, à part les lignes vides. Elles proviennent des lignes d'instructions {% %} qui sont terminées par un saut de ligne que Jinja2 nous retourne. Heureusement, utiliser un tiret après le pourcent permet de s'affranchir facilement de ce comportement :

    <table>
           <tr><th>name</th><th>location</th></tr>
    {%- for jedi in jedis %}
           <tr><td>{{ jedi }}</td><td>{{ jedis[jedi].location }}</td></tr>
    {%- endfor %}
    </table>

rend :

    <table>
    <tr><th>name</th><th>location</th></tr>
    <tr><td>obiwan</td><td>tatooine</td></tr>
    <tr><td>yoda</td><td>dagobah</td></tr>
    </table>

Si nos données sont structurées sous forme de listes :

    jedis:
	   - name: obiwan
	     location: tatooine
	   - name: yoda
	     location: dagobah

La variable jedi dans la boucle contient le hash composé des champs name et location. Aussi le template doit être modifié :
  
    <table>
    <tr><th>name</th><th>location</th></tr>
    {%- for jedi in jedis %}
        <tr><td>{{ jedi.name }}</td><td>{{ jedi.location }}</td></tr>
    {%- endfor %}
    </table>


Cette structure peut  également accepter des filtres. Par exemple, on peut limiter l'étendue de 

 

Un lien vers mon testeur de templates

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc5MjU4NDc5LDE1NjE4NTU5MjAsMzc0OT
EyMDY5LDUwNDcyMjc5MCwtOTg4NDY1NjA4LC0xNzkwNDI0NTMy
LC02NjE5OTAzMjYsMTYwMDI1NTA0NCwyMTI5MjM4NTc3LDQ5Nz
I4MDMzNSw3MzA5OTgxMTZdfQ==
-->