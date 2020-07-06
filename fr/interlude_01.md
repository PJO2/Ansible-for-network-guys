
# Interlude  : les formats de données complexes 
![enter image description here](https://github.com/PJO2/Ansible-for-network-guys/raw/master/images/YAML%20brick.png)
Avant d'aller plus loin dans la découverte d'Ansible, nous allons ouvrir une première parenthèse pour parler format de données et en particulier du langage YAML.

L'effort est nécessaire car les playbooks Ansible que nous allons étudier au prochain chapitre sont rédigés en YAML. Il est aussi libérateur, car il n'y a pas de bonne automatisation sans une structure de données adéquate.


## Le hash dans les langages récents

Les langages Perl et PHP ont popularisé un nouveau format de données : le tableau associatif (appelé encore hash, ou dictionnaire). C'est un ensemble de clefs associées à des valeurs, ou encore un tableau dont les indices sont des chaînes de caractères.

Par exemple, nous pouvons écrire

    adresse["obiwan"] = "tatooine"

ici, la variable est adresse, la clef est "obiwan", la valeur "tatooine". 


Ce nouveau type de données est devenu très rapidement populaire, il est intégré par tous les langages un peu récents sous différentes dénominations :

|  |  |
|--|--|
|Python  | dictionnary |
|Ruby| hash|
|Go|map|

Ces langages relèguent les tableaux classiques, à indices numériques, au rang de liste.

La valeur est de type quelconque et peut elle-même être une liste ou un tableau associatif, ce qui permet de travailler facilement et lisiblement avec des formats de données complexes.
Ici nous créons une valeur de type liste :

    padawans[obiwan] = [anakin, luke]

Ici, nous regroupons nos données en une seule variable structurée  :

    jedis["obiwan"]["padawans"] = ["anakin", "luke"]
    jedis["obiwan"]["location"] = "tatooine"
    jedis["obiwan"]["master"] = "qui-gon"

Le lecteur attentif aura noté la marque du pluriel (ie: le 's'), convention d'écriture qui permet de repérer plus facilement les listes et les tableaux des variables simples.
 

## Les formats YAML / JSON

Les formats YAML et JSON permettent de décrire des variables structurées en listes ou en tableaux associatifs dans un format texte, souvent un fichier.

Ansible a choisi le format YAML pour décrire la plupart des données, notamment pour sa lisibilité et sa compacité. 

Coté lisibilité, les niveaux d'imbrications sont représentés par l'indentation, chaque niveau d'indentation indique une sous-structure. Coté compacité,  la ponctuation est seulement composée des signes deux-points et tiret. Les accolades du JSON n'existent pas et les guillemets sont  optionnels.

Un changement d'indentation qui commence par un tiret est une nouvelle liste, sinon c'est une entrée clef/valeur supplémentaire de la variable ou de la liste en cours . 

Si nous reprenons nos premiers exemples, 
adresse["obiwan"] = "tatooine" s'écrit  : 

    adresse:
      obiwan: tatooine

 et padawans[obiwan] = [anakin, luke] s'écrit soit directement :
  

    padawans:
     - obiwan: [anakin, luke]

soit en créant un niveau d'indentation supplémentaire, ce qui, visuellement, correspond mieux à la complexité de nos données :

    padawans:
         - obiwan: 
              - anakin
              - luke


Nous pouvons représenter nos jedis par le fichier suivant :

    jedis:
        obiwan:
            master: qui-gon
            location: tatooine
            padawans:
             - anakin
             - luke
        yoda:
            padawans:
            - dooku
            - windu
            location: dagobah

Une autre représentation, pour laquelle les itération seront plus faciles,  utilise des listes  avec un label 

    jedis:
        - name: obiwan
          master: qui-gon
          location: tatooine
          padawans:
             - anakin
             - luke
        - name: yoda
          padawans:
            - dooku
            - windu
          location: dagobah

Enfin, il est préférable de débuter un fichier YAML par une première ligne composée de 3 tirets :

    ---
    
Cette marque indique le début d'un nouveau contenu YAML. Elle est implicite au démarrage d'un fichier, mais YAML prévoit qu'un fichier texte puisse regrouper plusieurs contenus.
 

## Structurer ses données

Alors que nous venons d'apprendre à décrire des données complexes, il est nécessaire de distinguer clairement le complexe du compliqué.

Le Robert donne les définitions suivantes :
> Complexe: qui contient, qui réunit plusieurs éléments différents
> Compliqué:   Qui est difficile à appréhender, analyser et comprendre. 

Bref, il est possible de faire moins compliqué en rendant plus complexe [mais dans tous les cas, on fera pas plus simple, car simple est l'antonyme de complexe et de compliqué, d'où les confusions !]. 

> Comme principe fondateur, on retiendra que la structure des données doit rendre leur utilisation la plus naturelle possible, tout en évitant au maximum leur redondance.

C'est dans cet objectif que les différents renseignements ont été regroupés dans la structure commune *jedis*, quitte à modifier l'ordre d'imbrication des données.


## Ecrire ses données 

Je vous déconseille d'écrire du YAML depuis un éditeur de texte, car les erreurs sont peu visibles (erreurs d'indentation,  oubli de l'espace entre le tiret et la clef, ...). En général, j'utilise un convertisseur en ligne YAML vers JSON, par exemple [https://www.json2yaml.com/](https://www.json2yaml.com/), qui vérifie la syntaxe et montre les données dans un autre format.

[jour 5](day_05.md)






<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1OTk2MzAxMiwxODQ5NzU3NjAsMTMxMT
MzNzI5MiwxMjcxODQzMDAzLC0xNjE0MTc0OTU3LC0zOTM2NTE5
ODAsMTAwODI3MDM0NCwtMTU4NjQ2MDc2MCwxNDAzODA4MDYsMj
E0NTU0NjY1MCwtOTQ5OTA5MDY3LC05NjE3MzU2MTAsOTcxNjM1
ODA3LC0xOTM2NjgzMTUsLTExOTM2NDU4MywtMTc0NDg5MjgyMC
wtMTAxMzA3NjgxNSwtMTMxMjM1NDI4OSwxNzUxNTk4NzAsLTEz
OTY0MzU2MV19
-->