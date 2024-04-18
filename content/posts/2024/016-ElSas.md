---
title: "El Sas"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - El Sas - Forensic - 100 points

## Énoncé
Un petit dump mémoire pour s'échauffer ! Trouvez le mot de passe de l'utilisateur `cycom`.

*Format flag :* **CYCOM{mot_de_passe}**

*Fichier attaché :* [`lsass.DMP`](/files/lsass.DMP)

## Solution
On est en face d'un dump du processus LSASS de Windows, qui est chargé de garder précieusement de mémoire certaines informations secrètes, commes des identifiants par exemple.

Pour analyser ce dump mémoire, on peut utiliser par exemple `mimikatz` avec sa suite de commandes `sekurlsa`.

```
mimikatz.exe
sekurlsa::minidump lsass.dmp
sekurlsa::logonPasswords full
```

![](/images/2024/016/01.png)

On obtient le hash NTLM de l'utilisateur `cycom`, il ne reste plus qu'a le casser à l'aide de `hashcat` ou de `john`.

Avec hashcat : 

```
hashcat -m 1000 "4180c3dccc2942b3a20e031af50ea714" rockyou.txt
```

![](/images/2024/016/02.png)

Avec john :
```
john ntlm.hash --wordlist=rockyou.txt --format=nt
```

![](/images/2024/016/03.png)

*Flag: CYCOM{get\*\*\*\*\*\*\*}*

## Auteur

Lyne