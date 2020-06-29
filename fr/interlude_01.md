
# Interlude  : les formats de données complexes 

Avant d'aller plus loin dans la découverte d'Ansible, nous allons ouvrir une première parenthèse pour parler format de données.  Cela nous permettra d'utiliser des données complexes dans nos scripts.

## Le hash dans les langages récents

Les langages Perl et Java ont popularisé un nouveau format de données : le tableau associatif (appelé encore hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs, ou encore un tableau dont les indices sont des chaînes de caractères.

Par exemple, on peut écrire

    adresse["obiwan"] = "tatooine"

ici, la variable est adresse, la clef est "obiwan", la valeur "tatooine". 

Ces langages relèguent les tableaux classiques, à indices numériques, au rang de liste.

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


Je vous déconseille d'écrire du YAML depuis un éditeur de texte, car les erreurs sont peu visibles (erreurs d'indentation,  oubli de l'espace entre le tiret et la clef, ...). En général, j'utilise un convertisseur en ligne YAML vers JSON, par exemple [https://www.json2yaml.com/](https://www.json2yaml.com/), qui vérifie la syntaxe et montre les données dans un autre format.

[jour 5](day_05.md)

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTgzMDQxMDQ2LDMzMjQ0ODE3MywtMTM2Mz
ExNTUzNywxMTAxNjgyMzk0LDE4OTg0NDg0MjgsNjUwMTk5Mjcy
LC0xMDgyNTEwNTg2LDE4NDA1OTU1MzcsLTI2NjI1Mjg0Niw0OT
E5MDY1OTEsLTU5ODcxODAzNSwzNDgyNzg5MjMsLTIwNjI5MzI5
Niw4NTU4Njc2NywxMDM2ODY5NTQ4LDcxNzI2MTk4Ml19
-->