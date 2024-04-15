---
title: "Unlucky Casino"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Unluky Casinco - Web - 300 points

## Énoncé
J'ai mis toute ma thune dans un coup de roulette dans ce casino mais ces malfrats ont fermé leur site juste avant que je retire mes gains !

Tu pourrais trouver le mot de passe de l'admin pour que je puisse récupérer mon dû ?

## Solution
A première vue, on a juste une page de connexion, rien de plus.

![](/images/2024/003/01.png)

On utilise `gobuster` pour tenter de trouver des routes plus intéressantes.

![](/images/2024/003/02.png)

On découvre un répertoire git exposé. Bonne nouvelle, on peut donc utiliser GitTools pour essayer de récupérer du code source.

![](/images/2024/003/03.png)

![](/images/2024/003/04.png)

En récupérant le contenu de `index.php`, nous sommes en mesure de comprendre comment l'authentification fonctionne.

![](/images/2024/003/05.png)

L'authentification se fait simplement sur une comparaison avec un hash hardcodé dans le code.

Si l'on regarde de plus près on constate deux choses : 
- La comparaison entre la variable `$_GET['pass']` et le hash est une comparaison "faible", elle compare après le transtypage
- Le hash du mot de passe de l'administrateur commence par `0e` et ne contient que des chiffres.

Lorsque ces deux conditions sont réunies, il est possible de contourner l'authentification en utilisant un hash magique, c'est une vulnérabilité connue de PHP.

Il suffit de prendre un des hashs magiques pour `sha224` [disponibles sur internet](https://github.com/spaze/hashes/blob/master/sha224.md) et de s'authentifier avec.

![](/images/2024/003/06.png)

![](/images/2024/003/07.png)

*FLAG : CYCOM{m4g\*\*\*\*\*\*\*}*

## Auteur

Lyne