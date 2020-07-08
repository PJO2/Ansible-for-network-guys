
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

## Les structures de boucles

Jinja2 possède un mécanisme de test et un mécanisme de boucle. Ces instructions sont placés entre les signes {% et  %}.

Par exemple, nous pouvons parcourir notre variable  jedis avec l'instruction :

     {% for jedi in jedis %} 
     {% endfor %}

Si nous utilisons notre base de données *jedis* sous la forme hash de hash :

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

La variable jedi dans la boucle contient cette fois le hash composé des champs name et location. Aussi le template doit être modifié :
  
    <table>
    <tr><th>name</th><th>location</th></tr>
    {%- for jedi in jedis %}
        <tr><td>{{ jedi.name }}</td><td>{{ jedi.location }}</td></tr>
    {%- endfor %}
    </table>



### Pourquoi cette différence ?
En python, comme dans de nombreux langages interprétés, les données sont typées et les instructions s'adaptent aux types des données.

Dans le premier cas, la variable *jedis* est un hash composé des clefs *obiwan* et *yoda* et itérer sur le hash renvoie les clefs une à une.
Dans le second cas, la variable *jedis* est une liste de hashs composés des clefs *name* et *location*. L'itération renvoie les hashs {name, location} un par un.

Dans le premier cas, l'instruction for peut également renvoyer le couple (clef, valeur) si  le hash est décomposé par la méthode *iteritems()*. Le template peut alors s'écrire :

    <table>
           <tr><th>name</th><th>location</th></tr>
    {%- for jedi,jedi_data in jedis.iteritems() %}
           <tr><td>{{ jedi }}</td><td>{{ jedi_data.location }}</td></tr>
    {%- endfor %}
    </table>

ou encore pour les plus audacieux, en jouant sur les noms de clefs et les valeurs, on arrive à un template qui apprend les champs à travers la structure de données  :

    <table>
        <tr><th>{{ (["name"] + jedis[jedis | first].keys()) | join('</th><th>') }}</th></tr>
    {%- for jedi,jedi_data in jedis.iteritems() %}
        <tr><td>{{ ([jedi] + jedi_data.values()) | join('</td><td>') }}</td></tr>
    {%- endfor %}
    </table>


## La structure de contrôle if
 
 Jinja2 possède une structure de test {% if %} {% endif %} qui fonctionne avec les opérateurs mathématiques (=, >,..), ensemblistes (in) ou logiques (true, false).

Ce petit exemple nous montre l'utilisation d'un premier if pour créer les en-têtes de tableaux seulement si la structure *jedis* n'est pas vide, puis limite l'affichage aux jedis obiwan et luke.

    {%- if jedis %}
    <table>
       <tr><th>name</th><th>location</th></tr>
	{%- for jedi in jedis %}
	   {%- if jedi.name in ["obiwan", "luke"] %}
       <tr><td>{{ jedi.name }}</td><td>{{ jedi.location }}</td></tr>
           {%- endif %}
        {%- endfor %}
    </table>
    {% endif %}

## Combiner filtres et instructions

Il est bien sûr possible d'utiliser les filtres à l'intérieur d'une instruction Jinja2. Ainsi, nous pouvons supprimer le test *if* ci-dessus en écrivant le template de la façon suivante :

    <table>
       <tr><th>name</th><th>location</th></tr>
    {%- for jedi in jedis | selectattr ("name", "in", "[obiwan, luke]") %}
       <tr><td>{{ jedi.name }}</td><td>{{ jedi.location }}</td></tr>
    {%- endfor %}
    </table>

ou pour un template 

    <table>
        <tr><th>{{ (jedis | first).keys() | join('</th><th>') }}</th></tr>
    {%- for jedi in jedis %}
        <tr><td>{{ jedi.values() | join('</td><td>') }}</td></tr>
    {%- endfor %}
    </table>

Un lien vers mon testeur de templates


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzMjg0NjYzMCwtMTMyMTkxOTkzMSwtMT
Q0Mzg4MjEyOSwzNTI3OTc2NjEsMTg5OTM0NzEwNCwtNDExMDU4
MTc2LC0xNTQ4MzkxNDUzLC0xNDk0NTU1MjMwLC0xODc2MzQxMz
M5LC03NDM4OTQ0MTYsMTU2MTg1NTkyMCwzNzQ5MTIwNjksNTA0
NzIyNzkwLC05ODg0NjU2MDgsLTE3OTA0MjQ1MzIsLTY2MTk5MD
MyNiwxNjAwMjU1MDQ0LDIxMjkyMzg1NzcsNDk3MjgwMzM1LDcz
MDk5ODExNl19
-->