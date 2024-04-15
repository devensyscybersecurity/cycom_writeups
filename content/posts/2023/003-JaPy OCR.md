---
title: "JaPy OCR"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - JaPy OCR - *300 pts*

## Énoncé
On m'a montré cette app géniale qui permet de transformer une image en texte, essaye !

*Attention, toutes vos entrées sont loggées*

*Le flag est stocké dans une variable d'environnement.*

*Ce challenge a été résolu par 4,2% des équipes.*

## Solution
On est en face d'une application d'OCR plutôt très sommaire : On peut submit une image avec du texte, le programme lit l'image et nous donne le résultat.

![](/images/2023/003/01.png)

JaPy.. Java Python ? Toutes nos entrées sont loggées ? Tout de suite je pense `Java + logs = Log4shell`.

Je tente donc d'utiliser [l'outil en ligne d'Huntress](https://log4shell.huntress.com/) pour les tests de log4shell. Pour ce faire, je sors mon bon vieux Paint, avec une police lisible facilement pour les OCR (ici, ça sera Verdana).

![](/images/2023/003/02.png)

Je l'envoie, je vérifie que la lecture est bien faite correctement, et je constate que Huntress reçoit bien une requête !

![](/images/2023/003/03.png)
![](/images/2023/003/04.png)

Selon l'énoncé, le flag est dans une variable d'environnement, ça tombe bien, car la syntaxe JNDI nous permet d'appeler la variable qu'on veut avec la syntaxe `${env:nom_var}`. Je vais donc chercher la variable `FLAG`.

![](/images/2023/003/05.png)

Encore une fois, je l'envoie, je vérifie que la lecture est bien faite correctement, et je constate que Huntress reçoit le flag directement dans l'outil !

![](/images/2023/003/06.png)
![](/images/2023/003/07.png)

*FLAG : opc{U0\*\*\*\*\*\*\*\*\*\*}*
