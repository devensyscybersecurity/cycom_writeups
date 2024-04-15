---
title: "Beep Boop Gallery"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Beep Boop Gallery - *300 pts*

## Énoncé

Je suis tombé sur ce site au hasard, j'ai bien fait mes recherches et je n'ai pas trouvé de fonctionnalité cachée. Pourtant je pense qu'il reste quelque chose d'autre à découvrir, mais tout ça me donne mal à la tête. Tu peux enquêter pour moi ?

**Ce challenge est fortement inspiré du wargame de `leHack 2022`.**

*Ce challenge a été résolu par 0% des équipes.*

## Solution

Il n'y a pas grand-chose sur ce site, juste une sélection de photos stock pas très intéressantes.

![](/images/2023/010/01.png)

Si on télécharge toutes ces photos et qu'on les analyse une par une, on ne trouvera rien non plus.

La clé de ce challenge n'est pas dans les images, mais directement dans les réponses du serveur.

À l'aide d'un outil type Burp Intruder, on peut facilement récupérer toutes les images d'un coup, et on observe que l'ID max est 20.

![](/images/2023/010/02.png)

Si l’on observe bien les réponses, on remarque une petite différence au niveau des headers : certains sont en majuscule tandis que d'autres non, et la capitalisation change selon l'ID de l'image requêtée.

![](/images/2023/010/03.png)
![](/images/2023/010/04.png)

En partant du premier header `x-frame-options` et en admettant que les 7 autres headers correspondent à un bit chacun, cela nous forme tout pile un byte par requête.

Si l’on prend `0` pour le lowercase et `1` pour le uppercase, on obtient par exemple :

![](/images/2023/010/05.png)

`01001111` en ASCII fait la lettre `O`.

Si on continue la même stratégie sur les autres IDs, on obtient :

```
01001111 01010000 01000011 01111011 01110011 01110100 00110011 01100111 00110000 01011111 01001000 01000101 00110100 01000100 00110011 01110010 01011010 01111010 01111101
```

On convertit en ASCII, et on obtient le flag.

![](/images/2023/010/06.png)

*FLAG : OPC{st3\*\*\*\*\*\*\*\*\*\*\*\*\*\*}*
