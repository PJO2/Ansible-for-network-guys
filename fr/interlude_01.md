
# Interlude  : les formats de données complexes 

Avant d'aller plus loin dans la découverte d'Ansible, nous allons ouvrir une première parenthèse pour parler format de données.  Cela nous permettra d'utiliser des données complexes dans nos scripts.

## Le hash dans les langages récents

Les langages Perl et PHP ont popularisé un nouveau format de données : le tableau associatif (appelé encore hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs, ou encore un tableau dont les indices sont des chaînes de caractères.

Par exemple, on peut écrire

    adresse["obiwan"] = "tatooine"

ici, la variable est adresse, la clef est "obiwan", la valeur "tatooine". 


Ce nouveau type de données est devenu très rapidement populaire, il est intégré par tous les langages un peu récents sous différentes dénominations :

|  |  |
|--|--|
|Python  | dictionnary |
|Ruby| hash|
|Go|map|

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
Coté lisibilité, les niveau d'imbrications sont représentés par l'indentation, chaque niveau d'indentation indique une sous-structure. Coté compacité,  la ponctuation est seulement composée des signes deux-points et tiret. Les accolades du JSON n'existent pas et les guillemets sont  optionnels.

Une indentation qui commence par un tiret est une nouvelle liste, sinon c'est une entrée clef/valeur supplémentaire de la variable ou de la liste en cours . 

Si nous reprenons nos premiers exemples, 
adresse["obiwan"] = "tatooine" s'écrit  : 

    adresse:
      - obiwan: tatooine

 et padawans[obiwan] = [anakin, luke] s'écrit soit directement :
  
padawans:
 - obiwan: [anakin, luke]
  
soit en créant un niveau d'indentation supplémentaire qui correspond à la complexité de nos données :

    padawans:
         - obiwan: 
              - anakin
                luke


Nous pouvons représenter nos jedis par le fichier suivant :

    ---
    jedis:
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


## Structurer ses données

Alors que nous venons d'apprendre à décrire des données complexes, il est nécessaire de distinguer clairement complexe et compliqué.
Mon Robert donne les définitions suivantes :
Complexe: qui contient, 

>  Qui est difficile à appréhender, analyser et comprendre. 

Je vous déconseille d'écrire du YAML depuis un éditeur de texte, car les erreurs sont peu visibles (erreurs d'indentation,  oubli de l'espace entre le tiret et la clef, ...). En général, j'utilise un convertisseur en ligne YAML vers JSON, par exemple [https://www.json2yaml.com/](https://www.json2yaml.com/), qui vérifie la syntaxe et montre les données dans un autre format.

[jour 5](day_05.md)


<!--stackedit_data:
eyJoaXN0b3J5IjpbNzIwMzUyODkxLDE3NTE1OTg3MCwtMTM5Nj
QzNTYxLDE1MTgyNTk0MiwtMTA4MjIzNTUyMywxNDMxNzA0Nzgz
LDExMDk4NjUzMjUsLTk2NDIyMTE1NywzMzI0NDgxNzMsLTEzNj
MxMTU1MzcsMTEwMTY4MjM5NCwxODk4NDQ4NDI4LDY1MDE5OTI3
MiwtMTA4MjUxMDU4NiwxODQwNTk1NTM3LC0yNjYyNTI4NDYsND
kxOTA2NTkxLC01OTg3MTgwMzUsMzQ4Mjc4OTIzLC0yMDYyOTMy
OTZdfQ==
-->