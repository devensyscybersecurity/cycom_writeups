---
title: "Inception"
date: 2023-06-13T09:03:20-08:00
draft: false
---


# OpenCycom 2023 - Inception - *300 pts*

## Challenge

Les exigences modernes en manière de sécurité, flexibilité et robustesse ont conduit les infrastructures informatiques à se complexifier. 
En effet, un simple serveur web n'est souvent pas accessible directement et passe par des intermédiaires. Ce que l'on appelle de reverse-proxy.

Ce site web en cours de développement n'est pas situé directement là où il semble être, et une authentification est utilisée sur le chemin réseau jusqu'au serveur web final.

*Ce challenge a été résolu par 4.2% des équipes.*

## Vulnérabilités

La fonctionnalité de debug HTTP `TRACE` est parfois activée, c'est le cas par défaut notamment sur le serveur Apache2. Cette méthode HTTP renvoie la requête telle qu'elle a été reçue. Si des informations d'authentification sont insérées par un intermédiaire dans notre requête HTTP, ces dernières nous seront retournées par cette fonctionnalité.

## Exploit

En lançant un scanner basique, comme nuclei, on remarque rapidement que la méthode `TRACE` est activée.

![](/images/002/00.png)


En utilisant la méthode `TRACE`, on remarque immédiatement que l'en-tête X-Forwarded-For est assez longue. On peut voir les adresses IP internes de chaque intermédiaire. Mais aucun secret n'est visible.

![](/images/002/01.png)

Ici, une difficulté supplémentaire a été ajoutée : L'information d'authentification n'est nécessaire uniquement et aie consommé par un proxy (intermédiaire) avant le serveur final. Elle n'arrive donc pas jusqu'au serveur web, qui ne nous l'affichera pas dans le retour du `TRACE`.


Cependant, une fonctionnalité offerte par le standard HTTP nous permet de contourner cette limite : "`Max-Forwards`". 
Ce paramètre nous permet de limiter le nombre de serveurs que notre requête traversera. Ce qui signifie que le `TRACE` s'arrêtera au serveur qui nous intéresse, et en incrémentant petit à petit cet en-tête, une copie de la requête reçue par chaque serveur reverse-proxy est obtenue.

![](/images/002/02.png)


Sur le quatrième serveur, le `TRACE` retourné contient bien le secret.
Utiliser `CURL` n'est pas obligatoire, un proxy d'attaque comme BURP permet également d'effectuer la manipulation :

![](/images/002/03.png)

