---
title: "CaCCool"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - CaCCool - *400 pts*

## Énoncé
Un petit exercice de reverse, à vos Ghidra !

## Solution
Deux solutions pour ce chall, une statique et une dynamique.

### Solution dynamique

C'est clairement la plus simple des deux, car il suffit d'utiliser `ltrace` pour voir tout appel vers des bibliothèques externes lors de l'exécution. Et donc le flag, lorsqu'il est traité par ces dernières. Cela est uniquement possible lorsque le programme utilise du linkage dynamique.

`ltrace ./caccool`

![](/images/018/01.png)

### Solution statique

Celle qui était suggérée par l'énoncé du challenge, et qui est un peu plus laborieuse : on a besoin de faire un peu de reverse engineering.

On s'équipe de `ghidra` et on ouvre l'exécutable avec pour lancer la décompilation.

Si on décompile le `main`, on se retrouve avec quelque chose comme ça : 

![](/images/018/02.png)

Deux choses intéressantes ici : on constate qu'il y a une boucle `while` qui effectue une opération XOR, ainsi qu'une comparaison avec l'entrée utilisateur.

On fait un peu de cleanup, on rajoute des noms de variable, et on se rend compte que la logique est loin d'être complexe : Le programme XOR deux ensembles de bytes ensemble, puis les compare à l'input de l'utilisateur. On peut donc retrouver le flag en retrouvant le contenu de ces deux bytearray.

![](/images/018/03.png)

Ghidra nous aide bien sur ce coup-là, car on peut facilement voir que les variables uStack sont simplement concaténées à la variable locale pour obtenir les clés, il nous suffit donc de passer un petit coup de Cyberchef pour récupérer les deux strings à XOR ensemble.

![](/images/018/04.png)

La première string est `z##5AJ5e659FD`

![](/images/018/05.png)

La seconde string est ```5s`N3yfUfj`t9```

![](/images/018/06.png)

On XOR les deux ensemble, et on obtient le flag!

![](/images/018/07.png)

*FLAG : OPC{r3\*\*\*\*\*\*\*\*\*\*}*
