---
title: "Usurpception"
date: 2023-06-13T09:03:20-08:00
draft: false
---


# OpenCycom 2023 - Usurpception - *500 pts*

## Challenge

Le challenge Usurpception se présente sous la forme d'un accès SSH à ce qui est une reproduction d'un serveur sous alpine Linux dans un conteneur docker. Plusieurs services (apache2 httpd, mariadb, fail2ba, ssh...) sont donc en cours de fonctionnement.

L'objectif est d'élever les privilèges de l'utilisateur fourni (devensept) et de lire le flag contenu dans un fichier à la racine du disque. Ce fichier n'est lisible que par root.

*Ce challenge a été résolu par 29,1% des équipes.*

## Vulnérabilités

La première vulnérabilité n'a aucun rapport avec ces services en fonctionnement, lors des recherches de composant vulnérables, on trouve que des fichiers sont disponibles dans le dossier usuel `/usr/local`. Ce dossier contient par convention des scripts spécifiques à la machine ou ce déploiement.

Dans ce dossier le fichier `/usr/local/bin/backup.sh` est un script de backup, le commentaire laisse supposer que le script est exécuté toutes les minutes. Le code du script exécute également tous les scripts situés dans le dossier `/usr/local/etc/backup_hooks.d/`. Après une inspection avec `ls` on ne remarque rien d'anormal et l'utilisateur courant ne semble pas y avoir les droits en écriture.

`ls` affiche le contenu du dossier, mais avec les options `-la`, il affiche également le contrôle d'accès et propriétaire des fichiers. Un indicateur sous la forme du signe `+` indique généralement si des ACL sont présentes. Cependant le `ls` sur Alpine Linux n'est pas celui de GNU Coreutils, cette version n'affiche pas cette indicateur.

Le système d'ACL (Access Control Lists) est un autre système venant compléter le système de contrôle d'accès traditionnel UNIX. Il permet un réglage beaucoup plus flexible et granulaire. Ici la vulnérabilité étant qu'un administrateur a attribué les droits d'écriture sur ce dossier à l'utilisateur devensept, probablement dans l'optique d'éviter l'utilisation de sudo, et pour faciliter la maintenance.

La seconde vulnérabilité vient de la mauvaise utilisation du système de capabilities sur un programme sensible. Les capabilities sont un mécanisme permettant de diviser les privilèges de root. En effet, cela permet par exemple d'assigner une partie des privilèges de root à une application, à un utilisateur... Quand les capabilities sont assignées à un fichier, le comportement est très similaire à mettre le programme en suid root. La seule différence est que le programme ne tournera pas en root, mais en tant que l'utilisateur appelant le programme, et qu'il possèdera en plus une fraction des privilèges de root spécifiés par les capabilities assignées. En théorie, c'est un mécanisme plus sécurisé, car le compte root n'est pas utilisé et les droits assignés sont plus faibles. Cependant, beaucoup de capabilities sont très dangereuses.

On remarque qu'un fichier `sec_tar` est présent dans `/usr/bin`. Ce fichier est possédé par l'utilisateur backupad, et seul ce dernier possède les droits de l'exécuter. Une inspection à l'aide du programme capsh indique que sec_tar possède la capability `CAP_DAC_OVERRIDE`. Cette dernière permet d'ignorer tout contrôle d'accès (DAC signifie Discretionary Access Control) sur les systèmes de fichiers. (oui, c'est bien un des privilèges de root). De la même manière que pour les suid binary, le problème vient que le programme ayant été octroyé cette capability n'est pas assez restrictif. Tar possède de nombreuses fonctionnalités dangereuses, et ne limite en rien les actions pouvant être effectuées avec. Il est donc possible d'exploiter cette capability de manière malveillante.


## Exploit

Les scripts dans `/usr/local/etc/backup_hooks.d/` sont exécutés par l'utilisateur `backupad`. Cela signifie donc qu'écrire un script dans ce dossier permet un déplacement horizontal et obtenir les droits du compte `backupad`.
Comme ce script est exécuté par cron, le plus simple est d'écrire le second stage de notre exploit dans ce script également.

Le second stage consiste à utiliser les fonctions de tar pour lire le fichier auquel notre utilisateur n'a pas accès. Il y a différentes possibilités, mais une très simple est de `tar` le fichier, puis d'inverser l'opération. Il est également possible d'utiliser les fonctions permettant d'exécuter du code de tar comme le montre GTFOBins
:

```bash
LFILE=/flag
tar xf "$LFILE" -I '/bin/sh -c "cat 1>&2"'
```



