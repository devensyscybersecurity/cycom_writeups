---
title: "Industrial Secrets (Partie 1)"
date: 2023-06-13T09:03:20-08:00
draft: false
---


# OpenCycom 2023 - Industrial Secrets (Partie 1) - *200 pts*

## Challenge

Le challenge simule un logiciel industriel rudimentaire sous la forme d'une console interactive d'administration. L'objectif est d'élever nos privilèges pour obtenir des données sensibles.

*Ce challenge a été résolu par 66,6% des équipes.*

## Vulnérabilités

On remarque qu'une taille limite est spécifiée pour le message de commentaire. Comme c'est une restriction, il est intéressant de vérifier qu'elle est correctement imposée. Ici ce n'est pas le cas, le code ne vérifie pas que la longueur est respectée et copie l'intégralité des données fournies.
Écrire un commentaire plus grand est possible, et les données superflues vont dépasser en mémoire et réécrire d'autres données.

Dans ce programme, un seul drapeau permet de définir si l'utilisateur a les privilèges d'administration ou pas. Les valeurs stockées sont positionnées de manière séquentielle en mémoire, c'est-à-dire qu'en dépassant le commentaire, il est possible de réécrire le rôle de l'utilisateur.


## Exploit

Il est possible d'ajouter un commentaire en utilisant la commande `setTechnicalNote`.

Ici le premier indicateur de la vulnérabilité est que la valeur de la valve change si le texte est trop long.

En ajoutant de plus en plus de données, on remarquera que le rôle de l'utilisateur sera passé à administrateur.

Ensuite, la commande `getStatsFTPConfig` permet d'obtenir les identifiants d'un FTP utilisé pour déposer des statistiques pour du monitoring. Ceci permet de passer au challenge suivant.

