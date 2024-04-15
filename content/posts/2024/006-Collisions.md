---
title: "Collisions"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Collisions - Crypto - 200 points

## Énoncé
Un petit exercice sur les collisions MD5, vous devez créer DEUX fichiers qui :
    - Contiennent le mot `CYCOM`
    - Ne sont pas identiques
    - Ont le même hash MD5

Une fois que vous avez les deux fichiers, mettez les dans le verificationatateur de collisionnations et récupérez le flag !

*Note: Ceci n'est pas un challenge web, n'attaquez pas la plateforme directement.*

## Solution
En faisant quelques recherches autour des outils qui existent pour générer des collisions MD5, on peut tomber sur la suite `HashClash`, et le programme `fastcoll`.

On télécharge l'outil, puis on crée un fichier `prefix.txt` dans lequel on écrit la string que l'on veut dans notre programme. (Ici `CYCOM`)

![](/images/2024/006/01.png)

Puis on génère les deux fichiers avec la commande :

```
fastcoll.exe -p prefix.txt
```

On obtient deux fichiers, `msg1` et `msg2` qui ont bien le même hash MD5, mais diffèrent de quelques bytes.

![](/images/2024/006/02.png)

On envoie les deux fichier dans le verificationatateur, et on récupère le flag.

![](/images/2024/006/03.png)

*FLAG : CYCOM{Md5\*\*\*\*\*\*\*}*

## Auteur

Lyne