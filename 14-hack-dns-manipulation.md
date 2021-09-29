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

L'objet de ce TP sera d'explorer les techniques pour exfiltrer et infiltrer les données ainsi que le Tunneling de DNS et comment il peut être utilisé pour bypasser différents protocoles comme HTTP via DNS.

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



**Installer iodine :**
```console
$ sudo apt-get install iodine
```

### Wireshark

Wireshark est un analyseur de paquets libre et gratuit. 

**Installer Wireshark :**
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
