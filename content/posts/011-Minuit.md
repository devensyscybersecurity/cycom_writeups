---
title: "Minuit"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Minuit - *300 pts*

## Énoncé
```Mon pote Thomas s'est récemment lancé dans la composition et la prod. Il m'a fait parvenir un extrait d'une mélodie qu'il utilisera pour son prochain album et me demande mon avis. Je trouve qu'elle sonne bizarre... pas vous ?```

+1 fichier attaché `minuit.mid`

## Solution

On est ici en face d'un fichier .mid, une inspection rapide avec file nous permet de voir qu'il s'agit d'un fichier MIDI, un format de description musicale qui comporte entres autres des notes avec leurs octaves, ainsi qu'une donnée temporelle, pour savoir quand elles sont jouées et à quelle vitesse.

Si vous avez déjà fait un peu de musique sur ordinateur, vous serez sans doute familier avec le format.

![](/images/011/01.png)

Ce qui est cool, c'est que le .mid, bien que ne contenant pas d'onde à proprement parler, est supporté par Audacity. On l'ouvre avec, et l’on peut voir une série de notes. Si l’on joue la piste, on se rend compte que les notes forment une mélodie un peu éclatée au sol.

Sans véritable cohérence musicale entre les notes, on peut se douter que chaque note est indépendante et représente une information atomique.

![](/images/011/02.png)

Si l'on prend cette suite de notes et qu’on les met à la suite, en séparant là où il y a un silence, on obtient :

```
E2 E1
F1 E2 A1 G1
E1 E3 F3
B1 A1 C1 A2 D1 E1 B1 G3 E3 E3 D4 F2 A3 E4 A1 D3 F3
```

Cela ressemble à une phrase, avec des mots séparés, où chaque note représenterait une lettre.
On peut déjà essayer de prendre toutes les notes situées à l'octave de base et de les représenter directement avec leur lettre. Cela nous donnerait :

```
X E
F X A G
E X X
B A C X D E B X X X X X X X A X X
```

Les 3 premiers mots pourraient donc être "LE FLAG EST". En regardant la substitution de la lettre L, S et T par exemple, il est possible de dresser un tableau qui prend en première ligne l'octave de base avec les lettres inchangées, puis qui revient à la ligne à chaque fois que la dernière colonne est atteinte. Cela donne : 

```
A B C D E F G
H I J K L M N
O P Q R S T U
V W X Y Z
```

Pour déchiffrer le texte complet, il nous suffit de prendre la colonne qui correspond à la note, puis de choisir la ligne en fonction de l'octave de base. Par exemple E2 donne L, C4 donne X, etc.

```
L E
F L A G
E S T
B A C H D E B U S S Y M O Z A R T
```

*FLAG : OPC{BAC\*\*\*\*\*\*\*ART}*
