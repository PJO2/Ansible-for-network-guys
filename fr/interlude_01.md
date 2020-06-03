
# Interlude  : les formats de données complexes 

Avant d'aller plus loin dans la découverte d'Ansible, je vous propose une parenthèse pour parler format de données. Le sujet n'est 

## Le hash dans les langages récents

Les langages Perl et Java ont popularisé un nouveau format de données : le tableau associatif (ou hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs. On peut le voir comme un tableau dont les indices sont des chaînes de caractères.

Par exemple, on peut écrire

    planete["obiwan"] = "tatooine"

ici, la clef est "obiwan", la valeur "tatooine". 

Les tableaux classiques, à indices numériques, sont relégués au rang de liste.

De plus, la valeur peut elle-même être une liste ou un tableau associatif, ce qui permet de travailler facilement et lisiblement avec des formats de données complexes.

    padawans[obiwan] = [anakin, luke]

On peut, par exemple, regrouper en une seule variable structurée nos données :

    jedis["obiwan"]["padawans"] = ["anakin", "luke"]
    jedis["obiwan"]["planete"] = "tatooine"
    jedis["obiwan"]["master"] = "yoda"
    

## Les formats YAML / JSON

Les formats YAML et JSON permettent d'enregistrer des variables structurées en listes ou en tableaux associatifs dans un fichier.

Ansible a choisi le format YAML pour la plupart des données, notamment pour sa plus grande compacité. 
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


Je vous déconseille d'écrire du YAML depuis un éditeur de texte, car les erreurs sont peu visibles (erreurs 'indentation,  




<!--stackedit_data:
eyJoaXN0b3J5IjpbOTMxMDgwMzAwLDY1MDE5OTI3MiwtMTA4Mj
UxMDU4NiwxODQwNTk1NTM3LC0yNjYyNTI4NDYsNDkxOTA2NTkx
LC01OTg3MTgwMzUsMzQ4Mjc4OTIzLC0yMDYyOTMyOTYsODU1OD
Y3NjcsMTAzNjg2OTU0OCw3MTcyNjE5ODJdfQ==
-->