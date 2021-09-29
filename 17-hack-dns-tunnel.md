---

### Tunneling : Theorie

- Dans les deux tâches précédentes, nous avons vu comment les requêtes et les réponses DNS pouvaient être utilisées pour infiltrer et exécuter des charges utiles. 

- Les entreprises auront généralement mis en place des pare-feu, des IDS (Intrusion Detection Systems) et/ou des IPS (Intrusion Protection Systems) afin de prévenir/alerter lorsque des protocoles entrants et sortants indésirables transitent par leur réseau. 

- Le protocole DNS est rarement surveillé par les entreprises. Pour cette raison, les pirates peuvent contourner un grand nombre de protocoles « indésirables » en utilisant le tunneling DNS.

-- 

### Tunneling : Exemple

Pour cet Exemple, nous allons explorer comment un pirate peut contourner divers sites Web restreints en utilisant HTTP sur DNS. Pour y parvenir, nous utiliserons a nouveau iodine. 

<img src="https://cdn.discordapp.com/attachments/798799811482353734/808200889432145950/tunnel.png"/>

**La configuration :**

- Une machine Linux hébergée par AWS qui sera le serveur de tunnel DNS.  
- une VM Ubuntu s'exécutant sur un ordinateur local qui sera le client du tunnel DNS.  
- un Nom de domaine public hébergé sur Google Domains (badbadtunnel.in)
- 
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

Maintenant, sur notre machine cliente, exécutez iodine avec les arguments suivants :

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
