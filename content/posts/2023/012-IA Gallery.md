---
title: "IA Gallery"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - IA Gallery - *100 pts*

## Énoncé

Je viens de développer mon site gallery, un ami m'a conseillé de mettre à jour mon serveur, mais tout fonctionne chez moi..

*Ce challenge a été résolu par 54,1% des équipes.*

## Solution

La clé de ce challenge est de ne pas se laisser emporter par toutes les images qu'on voit dans la galerie, elles ne servent à rien, car on est bien sûr un challenge web.

![](/images/2023/012/01.jpg)

Si on va faire un petit tour dans l'onglet `Réseau` de Chrome lorsqu'on recharge la page, on peut facilement voir les requêtes qui passent pour les analyser.

On peut remarquer qu'un header de version est présent, ce qui nous permet d'identifier la version de PHP qui fait tourner le site.

![](/images/2023/012/02.png)

Un petit tour sur Google nous permet de voir que cette version est une version backdoorée, qui nous permet d'obtenir très facilement une exécution de code sur le serveur.

![](/images/2023/012/03.png)

L'exploitation est très simple, il suffit de rajouter le header `User-Agentt: zerodiumsystem('[COMMANDE]');` pour faire exécuter au serveur la commande système de notre choix.

On peut le faire facilement à partir de Postman, curl, ou Burp Suite.

![](/images/2023/012/04.png)

À partir de là, on peut se balader tranquillement sur le système et trouver le flag dans `/root/flag.txt` !

![](/images/2023/012/05.png)

*FLAG : OPC{D4MN\*\*\*\*\*\*\*\*\*\*}*
