---
title: "Clippy"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Clippy - Crypto - 400 points

## Énoncé
Un ami m'a demandé de tester la premiere version de son application Clippy, un coffre à secrets en ligne avec un système d'authentification basé sur un chiffrement millitaire !
Il était tellement confiant dans la sécurité de son app qu'il m'a donné le code source de la version de demo.

J'ai un doute dans le choix de l'algorithme de chiffrement, tu pourrais vérifier ? 

Vole les secrets du compte "cycom".



## Solution
L'application Clippy nous permet de créer un nouveau coffre, ou bien de se connecter avec un token d'accès.

![](/images/2024/005/01.png)

Le processus d'inscription se fait uniquement avec un nom d'utilisateur, que l'on peut renseigner à notre guise.

![](/images/2024/005/02.png)

Une fois l'inscription terminée, notre nouveau coffre est affiché, avec un secret de demo.

![](/images/2024/005/03.png)

En observant les requêtes faites par l'application vers le serveur avec un proxy comme `Burp Suite`, on observe que le serveur génère un token d'accès lors de l'enregistrement, qu'il place ensuite en cookie.

![](/images/2024/005/04.png)

Mainenant, on regarde le code de l'application pour essayer de comprendre comment est généré et vérifié le token en question.

L'application ne comporte que 3 endpoints : 
 - `/api/register`
 - `/api/login`
 - `/api/secret`

L'endpoint `/api/login` est inutile, car il ne fait que reflèter l'entrée utilisateur sous forme de cookie, avant de rediriger vers la page des secrets, on peut donc l'ignorer.

![](/images/2024/005/05.png)

Le code qui génère le token se trouve dans l'endpoint `/api/register`.

![](/images/2024/005/06.png)

Le token est simplement un objet `JSON` contenant le nom d'utilisateur et le rôle de l'utilisateur, qui est ensuite chiffré avec `AES-128-ECB` et renvoyé encodé en `base64`.

**Mais c'est quoi AES-128-ECB ???**

`AES` est un algorithme de chiffrement par bloc. C'est à dire qu'il découpe la donnée à chiffrer en plusieurs morceaux de la même taille, et les chiffre tour à tour, dans l'ordre.

Dans le cas de `AES-128`, les blocs font 16 octets. Si le dernier bloc n'a pas la longueur exacte pour remplir le dernier bloc, on applique un algorithme de padding, qui vient combler l'espace vide avec une donnée prévisible pour finir le bloc.

Contrairement aux autres modes de AES plus récents comme `CBC`, qui se base sur le résultat du bloc précédent pour inialiser le chiffrement du prochain bloc, `ECB` chiffre avec la même clé de manière fixe à tous les blocs. Chaque bloc peut donc indépendament chiffré / déchiffré, sans avoir à connaître les blocs précédents.

Cela rend possible les attaque de type `Cut / Paste`, où lorsqu'un attaquant à la capacité de contrôler un ou plusieurs blocs entiers, il peut prendre ces blocs et les déplacer ailleurs pour altérer les données comme il le souhaite sans connaître la clé secrète.

Dans le cas du challenge, nous avons le contrôle sur ce qui est inséré dans l'objet JSON avant le chiffrement.

Si l'on prend le token généré pour l'utilisateur `test`, on peut voir qu'il a une longueur de 3 blocs.

![](/images/2024/005/08.png)

Dans l'idée de notre attaque `Cut / Paste`, il faudrait remplacer le dernier bloc `: "guest"}` par `: "admin"}`, et jouer avec le padding pour faire en sorte que lorsque les blocs 1 et 3 sont collés ensemble, on obtiene le bon nom d'utilisateur.

En s'aidant de la fonction `To Hexdump` de `CyberChef` pour mieux visualiser les blocs, on peut arriver à obtenir un nom d'utilisateur qui permet d'obtenir les bonnes données une fois coupé.

```
c: "admin"}      ycom
```
Si on simule l'attaque avec le texte clair, on obtient : 

![](/images/2024/005/10.png)

```
{"username" : "cycom", "status": "admin"}[6 CHARACTERES DE PADDING]
```

On sait que l'algorithme de padding utilisé par défaut par le module `crypto` est `PKCS#7`,
on va donc remplacer les 6 octets de padding dont nous avons besoin par `06 06 06 06 06 06`

![](/images/2024/005/11.png)

![](/images/2024/005/12.png)

![](/images/2024/005/13.png)

On a maintenant notre payload à insérer dans le champs utilisateur. On crée un nouveau compte : 

![](/images/2024/005/14.png)

On fait notre `Cut / Paste` sur le token qui est généré, on inverse le bloc 2 et le bloc 3, et on supprime le bloc 4.

![](/images/2024/005/15.png)

![](/images/2024/005/16.png)

On reconvertit en base64, et le tour est joué.

![](/images/2024/005/17.png)

*FLAG : CYCOM{AES\*\*\*\*\*\*\*}*

## Auteur

Lyne