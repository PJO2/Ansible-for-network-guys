
# Interlude  : les formats de données complexes 

Avant d'aller plus loin dans la découverte d'Ansible, nous allons ouvrir une première parenthèse pour parler format de données.  Cela nous permettra d'utiliser des données complexes dans nos scripts.

## Le hash dans les langages récents

Les langages Perl et PHP ont popularisé un nouveau format de données : le tableau associatif (appelé encore hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs, ou encore un tableau dont les indices sont des chaînes de caractères.

Par exemple, on peut écrire

    adresse["obiwan"] = "tatooine"

ici, la variable est adresse, la clef est "obiwan", la valeur "tatooine". 


Ce nouveau type de données est devenu très rapidement populaire, il est  intégré par tous les langages un peu récents :
Python	dictionnary
Go	map
Ruby	hash

Ces langages relèguent les tableaux classiques, à indices numériques, au rang de liste.

La valeur peut elle-même être une liste ou un tableau associatif, ce qui permet de travailler facilement et lisiblement avec des formats de données complexes.

    padawans[obiwan] = [anakin, luke]

On peut, par exemple, regrouper en une seule variable structurée nos données :

    jedis["obiwan"]["padawans"] = ["anakin", "luke"]
    jedis["obiwan"]["adresse"] = "tatooine"
    jedis["obiwan"]["master"] = "yoda"

Le lecteur attentif aura noté la marque du pluriel (ie: le s), qui permet de repérer plus facilement les listes et les tableaux des variables simples.
 

## Les formats YAML / JSON

Les formats YAML et JSON permettent de décrire des variables structurées en listes ou en tableaux associatifs dans un format texte, souvent un fichier.

Ansible a choisi le format YAML pour décrire la plupart des données, notamment pour sa lisibilité et sa compacité. 
En effet, les niveau d'imbrications sont représentés par l'indentation, chaque niveau d'indentation indique une sous-structure. De plus, les guillemets sont  optionnels.

Une indentation qui commence par un tiret est une nouvelle liste, sinon c'est une entrée clef/valeur supplémentaire. 

Nous pouvons représenter nos jedis par le fichier suivant :

    ---
    jedis :
        obiwan:
          - master: yoda
            location: tatooine
            padawans:
             - anakin
             - luke
        yoda:
          - padawans:
            - dooku
            - windu
            location: dagobah


Je vous déconseille d'écrire du YAML depuis un éditeur de texte, car les erreurs sont peu visibles (erreurs d'indentation,  oubli de l'espace entre le tiret et la clef, ...). En général, j'utilise un convertisseur en ligne YAML vers JSON, par exemple [https://www.json2yaml.com/](https://www.json2yaml.com/), qui vérifie la syntaxe et montre les données dans un autre format.

[jour 5](day_05.md)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQzMTg5NTc5MCwxNTE4MjU5NDIsLTEwOD
IyMzU1MjMsMTQzMTcwNDc4MywxMTA5ODY1MzI1LC05NjQyMjEx
NTcsMzMyNDQ4MTczLC0xMzYzMTE1NTM3LDExMDE2ODIzOTQsMT
g5ODQ0ODQyOCw2NTAxOTkyNzIsLTEwODI1MTA1ODYsMTg0MDU5
NTUzNywtMjY2MjUyODQ2LDQ5MTkwNjU5MSwtNTk4NzE4MDM1LD
M0ODI3ODkyMywtMjA2MjkzMjk2LDg1NTg2NzY3LDEwMzY4Njk1
NDhdfQ==
-->