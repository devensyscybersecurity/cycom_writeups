---
title: "BEGIWY"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - BEGIWY - Crypto - 300 points

## Énoncé

Déchiffrez le message suivant : `GGFGXGGVDVGDAVFAXXDFAAFGFXDVVVADDV`

![](/images/2024/022/01.png)

## Solution

### Etape 1 

Il faut rendre la grille lisible en changeant les symboles en morse, avec la lettre correspondante :

On découvre alors qu’il y a les mêmes lettres que les lettres qui chiffrent le message.

![](/images/2024/022/02.png)

### Etape 2 

Avec une rapide recherche internet on retrouve l’explication du chiffrement ADFGVX.

Pour déchiffrer le message on doit remplir un tableau de 7 colonnes car la clef est le mot “SYMBOLE”. 

Comme le cryptogramme comporte 34 lettres, il faudra 5 lignes. Cependant comme 5(lignes)x7(colonnes)=35 cases, il y aura (35 cases - 34 lettres) une colonne incomplète, qui ne comporta que 4 lignes.

On prépare donc un tableau, avec le mot-clef, puis, en-dessous, l'ordre des lettres dans l'alphabet. 

![](/images/2024/022/03.png)

On va remplir les colonnes avec le cryptogramme selon cet ordre de haut en bas, en prenant bien garde de ne pas remplir complètement la dernière colonne.

![](/images/2024/022/04.png)

On lit ensuite le tableau ligne par ligne pour retrouver l'antigramme : `FV FG FV GX AA GA GG DD XF AD VV DX GF AD VV DX GV.`

### Etape 3 

Il ne reste plus qu'à lire dans le tableau de chiffrement les lettres du message clair.

![](/images/2024/022/05.png)

La première lettre donne la ligne de la case du caractère à déchiffrer, la seconde donne sa colonne, au croisement des deux on retrouve la bonne lettre qui correspond au flag :

**FV** = *C* ; **FG** = *Y* ; **FV** = *C* etc… 

Le flag est donc : `CYCOM{GJPAINVAIN}`

## Auteure

Steak$ushi