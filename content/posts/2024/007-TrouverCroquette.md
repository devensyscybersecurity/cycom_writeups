---
title: "Trouver Croquette"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Trouver Croquette - Web - 100 points

## Énoncé
J'ai développé une nouvelle gallerie pour montrer mes super photos à la terre entière ! J'y ai mis quelques images, mais j'y ai également caché une photo de mon chat Croquette. J'utilise du stockage cloud, alors aucune chance que tu n'arrives à le trouver.... si ?

## Solution
Le site ne comporte qu'une page, qui charge six images différentes. Les images proviennent de Lorem Picsum, donc rien d'intéressant à voir là dedans.

![](/images/2024/007/01.png)

Si on s'intéresse au code source de la page, on voit que les images sont chargés depuis un conteneur de stockage Azure.

```
cycom2024.blob.core.windows.net/cycom2024
```

![](/images/2024/007/02.png)

Une vulnérabilité commune de ce genre de stockage en ligne peut apparaître lorsque l'administrateur du conteneur oublie de désactiver l'accès anonyme, ce qui permet à un attaquant potentiel de lister les données présentes dans le conteneur.

Cependant, au contraire de `AWS S3` par exemple, il ne suffit pas de revenir a la racine du conteneur pour afficher son contenu.

![](/images/2024/007/03.png)

Après une petite recherche, on trouve qu'il faut rajouter des paramètres dans l'URL pour pouvoir afficher le listing des données.

![](/images/2024/007/04.png)

En ajoutant les paramètres `restype=container&comp=list` à la racine du conteneur, on arrive à obtenir la liste des fichiers.

![](/images/2024/007/05.png)

Et on trouve Croquette.

![](/images/2024/007/06.png)

Il est pas trop choupitou quand même ?

*FLAG : CYCOM{cl0\*\*\*\*\*\*\*}*

## Auteur

Lyne