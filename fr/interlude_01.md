
# Interlude  : les formats de données complexes 

Avant d'aller plus loin dans la découverte d'Ansible, je vous propose une parenthèse pour parler format de données. Le sujet n'est 

## Le hash dans les langages récents

Les langages Perl et Java ont popularisé un nouveau format de données : le tableau associatif (ou hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs. On peut le voir comme un tableau dont les indices sont des chaînes de caractères.

Par exemple, on peut écrire

    mobile["aline"] = "0606060606"

ici, la clef est "aline", la valeur "0606060606". 

Les tableaux classiques, à indices numériques, sont relégués au rang de liste.

Toutefois, la valeur peut elle-même être une liste ou un tableau associatif, ce qui permet de travailler facilement avec des formats de données complexes.


## Les formats YAML / JSON



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg1NzcyMzU5NCwxMDM2ODY5NTQ4LDcxNz
I2MTk4Ml19
-->