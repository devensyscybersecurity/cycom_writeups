---
title: "Larousse"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Larousse - Forensic - 100 points

## Énoncé
J'ai fait une sauvegarde d'une photo... très importante... dans un zip chiffré il y'a quelques temps et j'ai perdu le mot de passe.

Tu pourrais m'aider à le retrouver ? Je crois qu'il était pas trop compliqué.

*Fichier attaché :* [`cycom.zip`](/files/cycom.zip)

## Solution
Allez, un zip chiffré pour se mettre en jambe. A la vue du titre et de l'énoncé, je pense qu'on peut commencer par une attaque dictionnaire classique.
  
![](/images/2024/013/01.png)

On récupère d'abord le hash du fichier avec `zip2john`, afin d'avoir un format utilisable pour les outils `hashcat` et `john`

Pour `hashcat` : 

```bash
zip2john cycom.zip | cut -d ':' -f 2 > cycom.hash
```

Pour `john` : 

```bash
zip2john cycom.zip > cycom.hash
```

Ensuite on lance un attaque par dictionnaire avec `rockyou.txt`.

Pour `hashcat` : 

```bash
hashcat cycom.hash rockyou.txt -m 13600
```

![](/images/2024/013/03.png)

Pour `john` : 

```bash
john cycom.hash --wordlist=rockyou.txt --format=zip
```

![](/images/2024/013/02.png)



Dans les deux cas, on tombe sur le bon mot de passe assez rapidement et on extrait l'image.

![](/images/2024/013/04.png)

![](/images/2024/013/05.png)

*Flag: CYCOM{z1p\*\*\*\*\*\*\*}*

## Auteur

Lyne