---
title: "Goat Games"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Goat Games - OSINT

## Partie 1 - 50 points

### Énoncé

Dans une clairière paisible, au cœur d'une prairie bordée de fleurs sauvages, un troupeau de chèvres naines gambadait joyeusement. Parmi elles, dix chèvres aux regards espiègles se détachaient, réunies par un lien mystique. Alors que le soleil déclinait à l'horizon, les dix chèvres se rassemblèrent en un cercle, leurs sabots formant des motifs dans la terre douce.

La première chèvre, du pelage blanc comme la neige, leva sa tête vers le ciel étoilé, murmurant "Abracadabra!" dans un souffle léger, tandis que des étincelles de magie scintillaient autour d'elle, illuminant la clairière de leur éclat.

La deuxième chèvre, aux cornes élégamment torsadées, ferma les yeux et prononça "p^piolmklSimsalabim!" d'une voix douce et mélodieuse, invoquant les forces mystiques de la nature, qui répondirent par un frisson dans l'air.

La troisième chèvre, plus vieille et sage que les autres, balança sa tête grisonnante et chuchota "Alakazampopù=!" avec une autorité calme, imprégnant l'atmosphère de sagesse ancestrale. Autour d'elle, les herbes se mirent à onduler dans une danse silencieuse.

La quatrième chèvre, aux yeux pétillants de malice, lança "Hocusp^pripoulus Pocus!" d'une voix enjouée, faisant tourbillonner des feuilles mortes dans un tourbillon magique.

La cinquième chèvre, au pelage tacheté, murmura "Presto!" en inclinant sa tête, et des fleurs écloses soudainement autour d'elle, éblouissant la clairière de leurs couleurs vives.

La sixième chèvre, la plus jeune du troupeau, cria "opùpo//opùo)^)$)$)$!" avec enthousiasme, provoquant un éclat de lumière étincelante qui éclaira la clairière d'une lueur dorée.

La septième chèvre, au regard perçant, susurra "NPMMdoe/a/moc.rugmi//:sptth!" d'une voix douce mais déterminée, faisant vaciller les ombres autour d'elle dans un jeu de lumière hypnotique.

La huitième chèvre, au tempérament calme, murmura "Shazamazdzrgrgregraza!" avec une tranquillité résolue, provoquant un frisson de magie qui parcourut le sol de la clairière.

La neuvième chèvre, aux cornes imposantes, lança "Klaatu!gtgtqsdsqcqBaradaNikto!" d'une voix puissante, faisant trembler légèrement le sol sous leurs sabots.

La dixième chèvre, la chef du troupeau, ferma les yeux et souffla "Sesameblablahyypms!" avec une autorité tranquille, enveloppant la clairière d'une aura protectrice.

**Quel est le nom de l'auteur de la photo ?**

Format du flag : `CYCOM{prénom_nom}`

### Solution

Nous pouvons voir que la 7ème chère délivre un sort, mais il s'agit d'une URL à l'envers. En la remettant droit, nous obtenons ce lien : 

```
https://imgur.com/a/eodMMPN
```

![](/images/2024/018/01.png)

On peut voir que l'auteur de la photo est `Jean RICARDO`

## Partie 2 - 100 points

### Énoncé

D'après la photo de `Jean RICARDO`, quel est le nom et le prénom de cet auteur célèbre ?

Format du flag : `CYCOM{prénom_nom}`

### Solution

Cette image contient un code encodé en `base64`.

En se concentrant uniquement sur la couleur bleue, il afut extraire les éléments dans l'ordre numérique (1>2>3).

La chaîne de caractères reconstituée est la suivante : 

```
aWwgZXN0IHVuIGF1dGV1ciBkZSBtYW5nYSBldCBjaGFyYWN0ZXIgZGVzaWduZXIgamFwb25haXMgbsOpIGxlIDUgYXZyaWwgMTk1NSDDoCBOYWdveWE=
```

Une fois décodée, elle révèle la phrase : « il est un auteur de manga et character designer japonais né le 5 avril 1955 à Nagoya ». 

Après recherche, il s'avère que cela concerne « Akira Toriyama ». 


## Partie 3 - 100 points

### Énoncé

On comprend que Jean RICARDO adore Dragon Ball. Comment se nomme le personnage de la photo 407 ?

### Solution

Pour obtenir davantage d'informations, il convient de se rendre sur le profil de Jean Ricardo, où l'on remarque la présence d'un ami nommé saiyajincy. En examinant les photos, on remarque qu'une d'entre elles est intitulée "407".

![](/images/2024/018/02.png)

L'image d'origine a été altérée en utilisant la fonction "twirl" sur le site en source ouverte https://www.photopea.com/.

![](/images/2024/018/03.png)

Pour restaurer l'image initiale, il faut appliquer la même fonction à l'image modifiée. Après des investigations supplémentaires, il a été déterminé que l'image représente "C-19".

## Partie 4- 100 points

### Énoncé

Quelle est la ville préféré de l’utilisateur « saiyajincy » ?

Après avoir identifié le compte Facebook sous le nom de "@saiyajincy", l'enquête se poursuit pour découvrir d'autres profils que l'utilisateur pourrait posséder sur d'autres réseaux sociaux, notamment Instagram.

![](/images/2024/018/04.png)

![](/images/2024/018/05.png)

Un autre compte potentiellement lié est celui nommé "saiyajin_cy", ce qui laisse supposer qu'il pourrait s'agir de la même personne. En scrutant les diverses photos de ce compte, une image d'un bâtiment est repérée, accompagnée du commentaire "ma ville préférée".

![](/images/2024/018/06.png)

![](/images/2024/018/07.png)

## Auteur 

Anonyme
