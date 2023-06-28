---
title: "Industrial Secrets (Partie 2)"
date: 2023-06-13T09:03:20-08:00
draft: false
---

# OpenCycom 2023 - Industrial Secrets (Partie 2) - *200 pts*

## Challenge

Un accès à un serveur FTP a été obtenu, est-il possible d'extraite de cet accès des informations sensibles ?

## Vulnérabilités

L'utilisation du FTP n'est pas sécurité de par son fonctionnement de base. L'ouverture de port est brute-forcable, et il est possible (selon le mode de fonctionnement actuel) de voler le fichier ou interchanger le fichier téléchargé. Si le chiffrement est activé, i est également possible que le chiffrement ne soit activé uniquement sur la partie contrôle et non données..

Cependant, la vulnérabilité à exploiter était plus simple encore : ce serveur FTP est multi-usage et contient d'autres fichiers sensibles. (mauvaise segmentation/séparation). Parmi ces fichiers, un est une archive ZIP chiffrée contenant un document sensible.
Le mot de passe en question est faible et a fuité sur internet.

## Exploit

Après une connexion au serveur FTP (par exemple en utilisant Filezilla), le fichier `Last_clients.zip` doit être téléchargé.

Ensuite, il est nécessaire de récupérer un dictionnaire de mot de passe. 
Dans ce cas, rockyou.txt est suffisant.

Il est disponible dans kali linux au chemin `/usr/share/SecLists/Passwords/Leaked-Databases/rockyou.txt.tar.gz` si le paquet seclists est installé, ou parfois `/usr/share/wordlists/rockyou.txt`. 
Il est également disponible sur différents repo github (ex: `danielmiessler/SecLists`).

Il faut ensuite extraire un hash cassable via ``zip2john Last_clients.zip | cut -d ':' -f 2 > hashes.txt``

Retrouver le mot de passe par force brute via hashcat en mode *23003* ( *SecureZIP AES-256* ) en utilisant *rockyou.txt* : ``hashcat -a 0 -m 13600 hashes.txt /usr/share/wordlists/rockyou.txt``

Extraire la feuille de calcul. Le FLAG est dans le champ du projet *Helios*.

