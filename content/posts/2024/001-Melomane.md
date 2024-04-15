---
title: "Métronome"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Métronome - Steganographie - 400 pts

## Énoncé
Je t'ai concocté une petite playlist avec toutes mes chansons préférées, dis moi ce que t'en penses.

https://www.youtube.com/playlist?list=PLt2IALVOaV0DJC6MdlxrhujNxoPZ2KxcO

## Solution

La clé de ce challenge ne figure pas dans les informations de la playlist ou à l'intérieur des vidéos en elles-mêmes, car je n'ai évidemment pas le contrôle sur le contenu.

Il s'agit simplement d'une technique de stéganographie basée sur les métadonnées des chansons que j'ai choisies, plus précisément le BPM de celles-ci.

En utilisant des outils en ligne exploitatant des bases de données publiques, il est possible d'obtenir facilement le BPM de toutes les chansons de la playlist.

![](/images/2024/001/01.png)

| Titre                  | BPM |
| :--------------------: | :-: |
| My Blood               | 109 |
| I Will Survive         | 117 |
| lovely                 | 115 |
| The Real Slim Shady    | 105 |
| Best Thing I Never Had | 99  |
| LoveGame               | 105 |
| Crazy Over You         | 115 |
| Anyone                 | 116 |
| Riders on the Storm    | 104 |
| Doing It Wrong         | 101 |
| Laverder Haze          | 97  |
| Butter                 | 110 |
| Dernière Danse         | 115 |
| bebis in the trap      | 119 |
| Die Hard               | 101 |
| successful             | 114 |

On remarque que l'étendue des BPM est relativement faible, et correspond presque exactement à la fouchette des codes ASCII pour les caractères en minuscule.

Une fois les BPM mis à la chaîne et convertis en ASCII, on obtient le message caché :

![](/images/2024/001/02.png)


*FLAG : CYCOM{mus\*\*\*\*\*\*\*}*
## Auteur

Lyne