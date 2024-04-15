---
title: "911 - What's your emergency ?"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - 911 What's your emergency ? - OSINT - 100 points

## Énoncé

Avec les informations que nous avons pu recueillir sur le vol, nous pouvons peu à peu retracé l’entourage de ce fameux voyageur !  
J’ai réussi à obtenir des informations car j’ai une amie qui bosse dans un aéroport… c’est toujours utile. Elle ne m’a pas posé de question mais elle doit se douter que je cherche quelque chose, tant pis. Je lui revaudrai ça.

Dans les données des passagers, il y avait son nom, prénom et un numéro de téléphone pour sa réservation : `+33644620189`. J’aimerais connaître son opérateur, pour voir si je peux récupérer son carnet d’adresses.  
Si j’ai bien compris, chaque opérateur possède un ou plusieurs “blocs”, un peu comme les adresses IP. Cela devrait t’aider.

Format du flag : `CYCOM{nomdelopérateur}`

## Solution

Comme énoncé, chaque opérateur possède des blocs de numéros, par exemple 0756, etc. L’Arcep, Autorité de Régulation des Communications Electroniques, des Postes et de la distribution de la Presse, recense et contrôle ces blocs, à la manière de l’ICANN pour les noms de domaine.

En se rendant sur le site de l’Arcep, il est possible de taper dans leur base de blocs pour trouver à quel opérateur appartient ce numéro en entrant les 6 premiers chiffres.

## Auteur

atz