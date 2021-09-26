## Nmap

<p align="center">
  <img src="https://img2.helpnetsecurity.com/posts/nmap-logo.jpg" />
</p>

- En piratage informatique, plus vous avez de connaissances sur un système ou un réseau cible, plus vous disposez d'options.<br/>
Nmap est installé par défaut sur les distributions Kali Linux.<br/>

- Nmap est accessible en tapant nmap dans la ligne de commande du terminal, suivi de certaines options, que nous verrons plus tard.<br/>
Tout ce dont vous aurez besoin pour cela est le menu d'aide pour nmap (accessible avec nmap -h) et/ou la page de manuel nmap (accessible avec man nmap).

Vous trouverez ci-dessous la documentation officielle :<br/>
https://nmap.org/docs.html

---

- Le But de ce premier TP est dans un premier temps d'explorer les options NMAP et de préparer votre boite à outils de commande 
- Cette boite à outils de commandes vous servira tout au long de la formation. Gardez la précieusement pour référence
- Dans un second temps, vous utiliserez certaines de ces commandes depuis votre machine d'attaque vers votre machine cible

- Les réponses aux questions inclues dans ce workshop sont à renseinger sur **le formulaire suivant** : 
                             
                                      https://forms.gle/C2XrfDZxqQVJr7B79
  
---

<p align="center">
  <img src="https://www.sparkpost.com/wp-content/uploads/2018/05/what-smtp-port-to-use_800x300.png"/>
</p>

Quels sont les composants réseaux pour le routage du traffic réseau vers les bonnes applications ?
```console
Réponse : 
```

Combien chaque machine accessble par réseau en possède-t-elle ?
```console
Réponse : 
```

Combien parmi eux sont considérés comme connus (ou standards) ?
```console
Réponse : 
```

---

## Les Options :
On part du principe que nous utilisons la version Linux de Nmap.
Quelle est la première option listée dans la documentation pour faire un "Syn Scan" ?

- Hint : 
```console
$ nmap -h | grep -i CE_QUE_VOUS_CHERCHEZ
```

```console
Réponse : 
```

Quelle option utiliser pour un "UDP Scan" ?

```console
Réponse : 
```

Afin de détecter quel système d'exploitation tourne sur la machine cible, quelle option utiliser ?

```console
Réponse : 
```

Nmap permet de connaitre les numéros de version des services tournant sur la machine cible. Quelle option le permet ?

```console
Réponse : 
```

Comment rendre les résultats des commandes nmap plus explicites ?

```console
Réponse : 
```

Comment augmenter d'un cran le niveau de verbosité de la commande nmap ?
```console
Réponse : 
```

Quelle option utiliser pour sauvegarder les résultats de la commande nmap dans trois formats différents ?

```console
Réponse : 
```

Quelle option utiliser pour sauvegarder les résultats de la commande nmap dans un format normal ?
```console
Réponse : 
```

Quelle option utiliser pour sauvegarder les résultats de la commande nmap dans un format sur lequel on peut appliquer une commande "grep" ?
```console
Réponse : 
```

Comment activer l'option "aggressive" pour obtenir davatange d'informations sur la machine cible ?

```console
Réponse : 
```

Comment calibrer la vitesse des scans exécutés par nmap au maximum ? <br/> Un scan rapide peut a plus de risque de générer des erreurs ou d'être 
```console
Réponse : 
```

Comment spécifier le port à scanner sur la machine cible ? Le port 80 par exemple.

```console
Réponse : 
```

Comment spécifier un intervalle de ports à scanner sur la machine cible ? Le port de 1000 à 1500 par exemple.
```console
Réponse : 
```

Comment dire à nmap de scanner tous les ports ?
```console
Réponse : 
```

Comment activer un script parmi les scripts de la librairie nmap ?

```console
Réponse : 
```

Comment activer les script de la catégorie "vuln" ?

```console
Réponse : 
```

---

## Les types de Scan :

Quelle RFC decrit le comportement standard du TCP ?
```console
Réponse : 
```

Si un port est fermé, quel flag sera retourné par le serveur pour l'indiquer ?
```console
Réponse : 
```

---

Quels sont les deux noms pour le "Sync Scan" ?
```console
Réponse : 
```

Est-il possible de faire un "Sync Scan" avec nmap sans avoir le rôle de super utilisateur ?
```console
Réponse : 
```

---

Lorsqu'un port UDP ne répond pas au scan de nmap, il est marqué comme étant ?
Essayez ce scan :
```console
$ nmap -sX -p1-999 -vv ADRESSE_IP
```
```console
Réponse : 
```

Lorsqu'un port UDP est fermé, par convention la cible renvoie un message "port unreachable" en réponse. Quel protocole fait cela ?
```console
Réponse : 
```

---

Parmi les scans NULL, FIN et Xmas utilise le flag URG ?
```console
Réponse : 
```

Dans quel but les scans NULL, FIN et Xmas sont ils utilisés ?
```console
Réponse : 
```

Quel système d'exploitation peut répondre avec un RST pour chaque port à un scan NULL, FIN ou Xmas ?
```console
Réponse : 
```

---

:construction: <br/>
Comment réaliser un balayage de ping sur le réseau 172.16.x.x (Netmask: 255.255.0.0) en utilisant nmap ?
```console
Réponse :
```

---

## Les scripts NSE (Nmap Scripting Engine) :

En quel langage sont écrits les script NSE (Nmap Scripting Engine) de nmap ?
```console
Réponse : 
```

:construction: <br/>
Quelle catégorie de scripts serait-il fort risqué d'exécuter en environnement de production ?
```console
Réponse : 
```

---

Quel argument optionnel le script ftp-anon.nse peut-il prendre ?
```console
Réponse : 
```
Liste d'utilisateurs anonymes à tester sur la cible.

---

Rechercher le mot clé "smb" dans le répertoire `/usr/share/nmap/scripts/` à l'aide des commandes bash appropriées.<br/>
Quel est le nom du fichier de script qui determine le système d'exploitation d'un serveur SMB ?<br/>
Astuce : Lancer une recherche sur smb puis os.
```console
$ grep smb /usr/share/nmap/scripts/script.db | grep -e '-os'
```

```console
Réponse : 
```

Lire le contenu du script rapidement et dire de quel autre script dépend-il ?
```console
$ grep dependencies /usr/share/nmap/scripts/smb-os-discovery.nse
```
```console
Réponse : 
```

---

## Firewalls :

Quel protocole est souvent bloqué par les machines cibles et nécessite l'option -Pn pour s'exécuter ?
```console
Réponse : 
```

Quelle option nmap permet de rajouter un nombre défini de données aléatoires à la fin des paquets ?

```console
Réponse : 
```

---

## Scan de la machine cible :

Est-ce-que la machine cible repond aux requêtes de type ping ICMP ?
```console
Réponse : 
```

Réaliser un scan de type Xmas sur les premiers 999 ports de la machine cible. Combien de ports sont marqués comme open ou filtered ?

```console
Réponse :
```

Comment expliquer cela ? L'utilisation de l'option -vv vous sera très utile.
```console
Réponse : 
```

Réaliser un scan de type TCP SYN sur les 5000 premiers ports de la machine cible. Combien de ports sont marqués comme open ?
```console
$ sudo nmap -xX -X n-nnn --xxxx -Xx ADRESSE_IP

[sudo] password for kali: 
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-18 21:00 CET
Nmap scan report for 10.10.37.8
Host is up (0.059s latency).
Not shown: 4995 filtered ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE
21/tcp   open  ftp
53/tcp   open  domain
80/tcp   open  http
135/tcp  open  msrpc
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 26.52 seconds
```
```console
Réponse : 
```

🚧 <br/>
Déployer le script ftp-anon. Le nmap peut-il se connecter avec succés au FTP sur le port 21 ?
```console
$ sudo nmap --xxxxxx xxx-xxxx -x nn ADRESSE_IP

Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-18 21:04 CET
Nmap scan report for 10.10.37.8
Host is up (0.069s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT

Nmap done: 1 IP address (1 host up) scanned in 31.29 seconds
```
```console
Réponse : 
```
