---
title: "Le Secret de la Tielle"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Le Secret de la Tielle - *200 pts*


## Challenge

Le challenge est sous la forme d'une image disque. L'objectif est de récupérer le contenu d'un fichier sensible contenu sur ce disque.

*Ce challenge a été résolu par 83,3% des équipes.*

## Solution

Dans un premier temps, l'image doit être extraite du zip. 


Testdisk peut ensuite être utilisé sur l'image. Ceci nous permet de voir que l'image contient un système de fichier ext4. 

![](/images/017/01.png)

![](/images/017/02.png)

![](/images/017/03.png)

![](/images/017/04.png)

Toujours avec testdisk, il est possible d'extraire les fichiers du disque, notamment le fichier `tielle.zip`. La navigation se fait de manière visuelle.

![](/images/017/05.png)

Le fichier semble corrompu. Malgré le fait qu'il est bien reconnu, impossible de l'extraire. 

![](/images/017/06.png)

En l'ouvrant le fichier avec un éditeur hexadecimal, on remarque que les magic bytes du zip ne sont pas correctes. Il est possible de les corriger, ou de simplement lire le flag en dessous.

![](/images/017/07.png)

Si l'on choisit de corriger le fichier, on peut ensuite l'extraire.

![](/images/017/08.png)



