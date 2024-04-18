---
title: "Catastrophe"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Catastrophe - RF - 300 points

## Énoncé
Depuis que j'habite aux Etats-Unis, je me suis mis à la radio amateur, c'est assez sympa et j'apprends plein de trucs.

Ce matin, des petits malins se sont amusés à diffuser un message d'alerte sur la fréquence 162.4 MHz, ils risquent gros. 

Peux tu décoder le message pour tenter de trouver sa provenance ?

*Format flag :* **CYCOM{Nom_de_la_station_emmetrice}**

*Fichier attaché :* [`recording.wav`](/files/recording.wav)

**IMPORTANT** : Ce signal ne doit être diffusé **SOUS AUCUN PRETEXTE** et encore moins sur le territoire américain. La diffusion d'un signal d'alarme factice tel que celui-ci est une très mauvaise idée et est probalement (je suis pas expert juridique, lol) puni par la loi. Ni l'auteur, ni Devensys Cybersecurity ne peuvent être tenus responsables en cas de diffusion de ce message.

## Solution
Le message audio semble se découper en quatre parties. La première est une série de trois bruits rappellant un modem des années 90, ensuite une tonalité fixe, ensuite un message, et pour finir une série de trois bruits plus courts.

![](/images/2024/008/01.png)

L'auteur de l'ennoncé nous evoque la fréquence `162.4 MHz`, ce qui nous permet de rechercher ce qui est normalement diffusé dessus.

Après une recherche, on trouve que c'est la fréquence du `NWR (NOAA Weather Radio)`, une station radio américaine d'urgence et automatique, chargée de relayer les alertes de l'EAS `Emergency Alert System`.

![](/images/2024/008/02.png)

Si l'on écoute des exemples de messages diffusés sur cette radio, on tombe sur plus ou moins la même structure de message, on est donc sur la bonne piste.

Il faut maintenant trouver l'encodage utilisé.

En faisant des recherches sur l'EAS, on trouve que les 3 bruits que l'on entend sont l'entête `SAME` du message d'urgence, qui contient des informations comme le type d'alerte, la zone géographique concernée, ou encore la station émmetrice ;)

![](/images/2024/008/03.png)

![](/images/2024/008/04.png)

On cherche donc un outil pouvant décoder l'entête SAME d'un enregistrement audio d'une alete EAS, afin d'en trouver la provenance.

Plusieurs options sont possible, pour ma part j'ai trouvé l'outil [samedec](https://crates.io/crates/samedec), qui est capable de prendre de l'audio en entrée.

Dans la documentation de `samedec`, il y a un exemple pour faire exactement ce qu'on veut faire :

```bash
sox 'Same.wav' -t raw -r 22.05k -e signed -b 16 -c 1 - | samedec -r 22050
```

Avant d'executer la commande, il faut d'assurer d'avoir renseigné la bonne fréquence d'échantillonage. Pour cela on peut ouvrir le fichier avec Audacity, et le fréquence d'échantillonage sera affichée.

![](/images/2024/008/05.png)

La fréquence est de 24000Hz, on refait la commande avec la bonne fréquence : 

```bash
sox 'Same.wav' -t raw -r 24k -e signed -b 16 -c 1 - | samedec -r 24000
```

On obtient la version décodée de l'entête. Il suffit de regarder les derniers charactière pour obtenir le callsign de la station emmétrice.

![](/images/2024/008/06.png)

```
ZCZC-PEP-EAN-509003+0000-1230530-EASISFUN-
```

*FLAG : CYCOM{EAS\*\*\*\*\*\*\*}*

## Auteur

Lyne