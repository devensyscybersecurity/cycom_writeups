---
title: "Méchant USB"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Méchant USB - Forensic - 300 points

## Énoncé
J'ai laissé malenconteusement dévérouillé pendant que j'étais aux toilettes dans mon café de quartier. 10 minutes plus tard, j'avais perdu l'accès à mon compte Discord.

J'ai trouvé une grosse clé USB par terre un peu plus loin. J'ai ensuite capturé les paquets envoyés par la clé avec Wireshark.

Tu pourrais arriver à décoder le contenu et à trouver le malware ?

*Fichier attaché :* [`capture.pcapng`](/files/capture.pcapng)

*Format flag :* **CYCOM{hash_md5_du_malware}**

## Solution
On se retrouve avec un fichier de capture de paquets qu'on peut ouvrir avec Wireshark.

![](/images/2024/010/01.png)

On voit des paquest dont le protocole est `USB`, jusque là, rien d'inattendu.

Les premiers paquets servent à déterminer, au moment du branchement de l'appareil, de quelle nature, quelle marque, quel modèle, etc. il est pour pouvoir charger le driver adéquat et ainsi interpréter la suite de la conversation correctement.

Ce qui nous intéresse, c'est surtout les paquets `USB_INTERRUPT in`, qui représentent les informations envoyés par la clé à l'ordinateur de la victime.

On remarque que tous les paquets `USB_INTERRUPT in` proviennent de l'adresse `2.3.1`. Lorsqu'on regarde plus haut dans la conversation, on peut retrouver les paquets de description, qui nous permettent de voir de quel genre d'appareil il s'agit.

![](/images/2024/010/02.png)

Une clé USB détectée comme un combo clavier + souris Logitech ? Tout ça sent fort le BadUSB.

Maintenant que l'on connait la nature du device, on peut commencer à regarder le contenu des paquets `USB_INTERRUPT in`

![](/images/2024/010/03.png)

On peut ignorer l'entête USB pour se concenter uniquement sur le `HID Data` envoyé par la clé USB.

Ici, chaque paquet représente une frappe de clavier. Le paquet se découpe comme tel sur 9 octets :
- Le premier octet est toujours a `01`, il peut être ignoré
- Le second octet représente quel modifier (CTRL, ALT, etc) est appuyé. Si rien n'est appuyé, il est a `00`
- Le troisième octet est toujours nul. `00`
- Les octets 4,5,6,7,8 et 9 réprésent les touches appuyées. Chaque octet représente une touche, et il est possible d'appuyer sur 6 touches maximum en meme temps.

En premier lieu, il faut extraire les données qui nous intéresse, a savoir chaque partie `HID Data` des paquets `USB_INTERRUPT in`. Pour faire ça facilement, on peut utiliser `tshark`.

```bash
tshark capture.pcapng -Y "usbhid.data" -T fields -e usbhid.data 2> /dev/null > keystrokes
```

On se retrouve avec un fichier qui contient chaque paquet séparé par un retrour ligne.

![](/images/2024/010/04.png)

On peut ensuite utiliser un script qui permet de récupérer les frappes depuis ce genre de données, en transformant les `scancodes` du clavier en charactères.

Le seul souci est que le clavier de notre ami est en AZERTY, il faudra donc adapter le script qu'on utilise pour retrouver le texte tapé.

Un exemple de script qui fonctionne : 

```python
# coding=utf-8
usb_codes = {
    0x04:"qQ", 0x05:"bB", 0x06:"cC", 0x07:"dD", 0x08:"eE", 0x09:"fF",
    0x0A:"gG", 0x0B:"hH", 0x0C:"iI", 0x0D:"jJ", 0x0E:"kK", 0x0F:"lL",
    0x10:",?", 0x11:"nN", 0x12:"oO", 0x13:"pP", 0x14:"aA", 0x15:"rR",
    0x16:"sS", 0x17:"tT", 0x18:"uU", 0x19:"vV", 0x1A:"zZ", 0x1B:"xX",
    0x1C:"yY", 0x1D:"wW", 0x1E:"&1", 0x1F:"é2", 0x20:"'3", 0x21:"'4",
    0x22:"(5", 0x23:"-6", 0x24:"è7", 0x25:"_8", 0x26:"ç9", 0x27:"à0",
    0x2C:"  ", 0x2D:")°", 0x2E:"=+", 0x2F:"[{", 0x30:"]}",  0x32:"#~",
    0x33:"mM", 0x34:"'\"",  0x36:";.",  0x37:":/"
    }

lines = ["","","","",""]

pos = 0

for x in open("keystrokes","r").readlines():
    code = int(x[6:8],16)

    if code == 0:
        continue
    # newline or down arrow - move down
    if code == 0x51 or code == 0x28:
        pos += 1
        continue
    # up arrow - move up
    if code == 0x52:
        pos -= 1
        continue

    # select the character based on the Shift key
    if int(x[2:4],16) == 2:
        lines[pos] += usb_codes[code][1]
    else:
        lines[pos] += usb_codes[code][0]


for x in lines:
    print (x)

```

On execute le script, et on retrouve ce qui a été frappé par le BadUSB !

![](/images/2024/010/05.png)

Le BadUSB fait donc un simplement téléchargement + execution d'un script bash.

On le télécharge et on calcule son empreinte MD5 pour valider le challenge !

```bash
wget http://mal.challengecyber.fr/hE_qkize -O - | md5sum

a8ae43bfe171377c972dbea39f2cc92d
```

*Flag: CYCOM{a8ae\*\*\*\*\*\*\*}*

## Auteur

Lyne