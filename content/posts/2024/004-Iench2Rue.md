---
title: "Iench2Rue"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Iench2Rue - Web - 300 points

## Énoncé
J'ai une mission urgente et d'ordre personnel pour toi. Mon chien, Tobby, s'est fait enlever il y a une semaine par deux hommes cagoulés.

J'ai fait ma petite enquête, et j'ai trouvé pourquoi ils font ca: ils kidnappent des chiens de race dans la rue et ils les revendent par l'intermédiaire d'un site internet.

J'ai pu identifier l'adresse du site, mais je n'y vois pas grand chose à part la liste des chiens à vendre.

Aide-moi à retrouver mon Tobby, c'est dans tes cordes non ?

## Solution

![](/images/2024/004/01.jpg)

Pour trouver la solution de ce challenge, il faut déjà identifier la stack techno employée par le site, car la vulnérabilité se trouve dans l'implémentation du framework.

En générant une erreur 404, j'obtiens une erreur stylysée, et en faisant une recherche sur son texte, j'en conclus qu'il s'agit d'une erreur du framework Symfony.

![](/images/2024/004/02.png)

![](/images/2024/004/03.png)

Il existe plusieurs vulnérabilités autour du framework Symfony, mais celle qui va nous intéresser est la [RCE _fragment](https://www.ambionics.io/blog/symfony-secret-fragment)

Lorsque l'endpoint `_fragment` est activé, il est possible, moyennant de connaître le `APP_SECRET` de l'application, d'obtenir une execution de code à distance sur le serveur.

Si l'on requête l'endpoint `_fragment`, nous recevons `403` en réponse, ce qui prouve qu'il est bien activé.

![](/images/2024/004/07.png)

On cherche donc un `APP_SECRET`, que l'on peut trouver.

En effectuant un peu de fuzzing sur l'app, on arrive à trouver une page `phpinfo.php`.

![](/images/2024/004/04.png)

Et coup de bol, le token `APP_SECRET` figure parmi les variables d'environnement exposées sur le PHPInfo.

![](/images/2024/004/05.png)

Avec ces infos, on peut maintenant utiliser l'endpoint `/_fragment` pour obtenir une execution de commande sur l'application. Il existe des scripts qui font pratiquement tout à notre place, comme par exemple [`symfony-secret-fragments-v2.py`](https://github.com/ambionics/symfony-exploits/blob/main/symfony-secret-fragments-v2.py)

On peut vérifier que notre exploitation fonctionne : 

```bash
python ./symfony-secret-fragments-v2.py https:/iench2rue.challengecyber.fr/_fragment -s 'mcPYaKwbvD2YVje5akrBKGPcYKVIKwqT'
```

![](/images/2024/004/06.png)

Et on vient ensuite tester une commande système : 

```bash
python ./symfony-secret-fragments-v2.py https:/iench2rue.challengecyber.fr/_fragment -s 'mcPYaKwbvD2YVje5akrBKGPcYKVIKwqT' --function 'system' --arguments 'command:id' -m 1
```

![](/images/2024/004/08.png)

En visitant le lien donné par le script, notre commande d'execute.

![](/images/2024/004/09.png)

Il ne nous reste plus qu'a récupérer le flag !

`ls /` :
![](/images/2024/004/10.png)

`cat /flag.txt` :
![](/images/2024/004/11.png)

*FLAG : CYCOM{sYm\*\*\*\*\*\*\*}*

## Auteur

Lyne