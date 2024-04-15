---
title: "Chaudière as a service"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Chaudière as a service - *200 pts*

## Challenge

La page du challenge représente une page de statistiques d'une chaudière. Ces données sont stockées sur une base de données externe, l'objectif est d'extraire des informations sensibles de cette même base de données avec comme seul point d'entrée, cette page et son paramètre ID.

*Ce challenge a été résolu par 0% des équipes.*

## Vulnérabilités

La vulnérabilité présente est une injection SQL. Cependant, injecter du SQL directement ne fonctionne pas.

## Exploit

On remarque que les données retournées sont URL-encodé. Cela peut laisser supposer que cette page est un proxy pour la page originale d'où l'encodage.

En partant sur ce principe, peut être que l'encodage altère notre injection, ou encore qu'un filtrage est effectué. Si un filtrage est effectué, est-il fait avant ou après le désencodage.
Dans le premier cas ou le second où le filtrage est effectué avant, double-url encoder notre charge utile permettra de la protéger.

![](/images/2023/008/01.png)

En utilisant SQLMap, l'option --tamper permet d'appliquer des transformations et obfuscations sur la charge utile. Nous pouvons utiliser le script `--tamper=chardoubleencode`

![](/images/2023/008/02.png)

L'injection est bien détectée et présente.

![](/images/2023/008/03.png)

Nous pouvons extraire les secrets contenus dans d'autres tables.
Pour lister les tables, l'option `--tables` peut être utilisé.

![](/images/2023/008/04.png)

Pour tout récupérer, l'option `--dump all` est utilisé.
Le flag est dans la dernière table extraite.

![](/images/2023/008/05.png)



