---
title: "Chatty Chatty"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Chatty Chatty - Web - 200 points

## Énoncé

Je t'incombe la tâche de réaliser un exposé sur l'espace. T'inquiète pas, je t'ai laissé une super resource pour aller plus vite : un chatbot spécialiste !

*La brindille se plie mais ne rompt pas*

## Solution

La résolution de ce défi réside dans l'interaction avec le chatbot mis à disposition, qui était relié à un modèle LLM (Large Language Model).

La première étape consistait à énumérer les fichiers et répertoires présents sur le site. En le faisant, nous avons remarqué la présence du fichier index.php, qui se trouve être le fichier racine et nous indique donc la technologie utilisée, à savoir du PHP.

![Technologie](/images/2024/014/01.png)

En parcourant le site, nous avons remarqué qu'il était possible d'interagir avec le chatbot.

![Interaction chatbot](/images/2024/014/02.png)

La première chose que l'on souhaite tester lorsqu'on voit un champ utilisateur sont les injections.

Lorsqu'on envoie une XSS basique, nous ne remarquons pas grand-chose, **à part** le fait que le bot traite notre requête. Ce dernier, par nature, modifie nos payloads car il essaie de construire une réponse logique avec ce que nous lui envoyons.

![XSS](/images/2024/014/03.png)

Nous lui renvoyons donc le même payload, mais cette fois-ci en lui précisant de ne pas interférer avec ce que nous lui envoyons.

![Be smarter](/images/2024/014/04.png)

Néanmoins, une XSS ne semble pas être la solution car notre interaction avec le bot semble se limiter à notre session. Lorsque nous rafraîchissons la page, toute la conversation est effacée, donc rien ne semble être stocké.

Nous pouvons donc nous concentrer sur un autre type d'injection : les SSTI (Server-Side Template Injection).

![SSTI basic](/images/2024/014/05.png)

Comme on peut le constater sur la capture d'écran précédente, le bot a bien renvoyé 49, ce qui correspond au résultat attendu. **Cependant**, cela ne prouve pas forcément qu'il y a une SSTI, car le bot pourrait avoir lui-même renvoyé 49 (en l'interprétant) et donc modifié notre payload avant qu'il ne soit renvoyé au site.

Pour confirmer la présence d'une SSTI, il faut donc envoyer des payloads plus complexes.

Dans cette optique, nous avons essayé tous les payloads des moteurs de templates PHP, et nous avons réussi à obtenir une réponse pour le moteur **Twig**, ou "brindille" en français =).

![/etc/passwd](/images/2024/014/06.png)

En affichant le contenu de */etc/passwd*, nous pouvons constater que l'utilisateur JEFF existe et qu'il possède un répertoire personnel */home/JEFF*.

Nous pouvons donc partir du principe que le flag se trouve dans ce répertoire.

![/home/JEFF](/images/2024/014/07.png)

Il ne nous reste plus qu'à l'afficher.

![Flag](/images/2024/014/08.png)


*FLAG : CYCOM{cH\*\*\*\*\*\*\*}*

## Auteur

Bardo