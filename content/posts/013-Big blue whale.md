---
title: "Big blue whale"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Big blue whale - *100 pts*

## Challenge

Un accès à un repository interne d'image docker a été obtenu. Quelles données peuvent être extraites.

## Vulnérabilités

Les développeurs ont supposé que les métadonnées de l'image du conteneur docker, dont les étapes de constructions, étaient secrètes ou illisibles. Or ce n'est pas le cas. Si des variables d'environnement sont assignées dans le Dockerfile même temporairement, elles sont visibles en tant qu'étape.

## Exploit

Dans un premier temps, télécharger l'image avec `docker pull`.

La commande `docker inspect` permets de récupérer les informations de l'image.

Le flag est visible dans les informations de l'image, il est précédé par `FLAG` :

```bash
$ docker inspect test | grep FLAG
ENV FLAG=OPC{TH3_WH4LE_I5NT_THAT_SECUR3}
```



