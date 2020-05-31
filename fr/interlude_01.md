
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

On peut, par exemple, regouper en une seule variable structurée nos don :


## Les formats YAML / JSON

Les formats YAML et JSON permettent d'enregistrer des variables structurées en listes ou en tableaux associatifs dans un fichier.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU2NDkwMTE2MCwtNTk4NzE4MDM1LDM0OD
I3ODkyMywtMjA2MjkzMjk2LDg1NTg2NzY3LDEwMzY4Njk1NDgs
NzE3MjYxOTgyXX0=
-->