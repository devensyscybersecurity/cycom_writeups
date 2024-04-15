---
title: "GLPI - One More Time"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - GLPI - One More Time - Web - 300 points

## Énoncé
Un administrateur a encore décidé d'utiliser GLPI. Essayez de récupérer un fichier "flag.txt" à la racine du système.

## Solution

En arrivant sur un GLPI en version 10+, nous observons que l'accès au répertoire `/files` est interdit avec une erreur 403.

![](/images/2024/009/01.png)

Cependant, en examinant le répertoire `/plugins`, nous remarquons qu'un plugin nommé "barcode" est installé.

![](/images/2024/009/02.png)

Ce plugin, dans certaines versions, est vulnérable à une path traversal. Il ne reste plus qu'à l'exploiter !

![](/images/2024/009/03.png)

Le flag est situé à la racine du système.

*FLAG : CYCOM{Y0UW*********}*

## Auteur

Nasty