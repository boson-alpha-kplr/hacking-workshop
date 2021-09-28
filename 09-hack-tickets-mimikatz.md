## Passage de Ticket Kerberos avec Mimikatz

Mimikatz est un outil de post-exploitation très populaire et puissant le plus couramment utilisé pour dumper les informations d'identification des utilisateurs à l'intérieur d'un réseau Active Directory. 
Mais Il peut également être utilisé pour dumper un TGT de la mémoire LSASS.

Vous pouvez exécuter cette attaque sur la machine fournie, mais vous passerez d'un administrateur de domaine à un administrateur de domaine en raison de la façon dont le contrôleur de domaine est configuré (mode démo)

**Passage de ticket** 

- Le passage de ticket fonctionne en dumpant le TGT de la mémoire LSASS de la machine. 
- Le service de sous-système d'autorité de sécurité locale (LSASS) est un processus de mémoire qui stocke les informations d'identification sur un serveur Active Directory et peut stocker un ticket Kerberos avec d'autres types d'informations d'identification pour agir en tant que garde-porte et accepter ou rejeter les informations d'identification fournies. 
- Vous pouvez dumper les tickets Kerberos de la mémoire LSASS tout comme vous pouvez dumper les hachages. 
- Lorsque vous dumpez les tickets avec mimikatz, cela nous donnera un ticket .kirbi qui peut être utilisé pour gagner l'administrateur de domaine si un ticket d'administrateur de domaine est dans la mémoire LSASS. 

- Cette attaque est idéale pour l'escalade de privilèges et le mouvement latéral s'il existe des tickets de compte de service de domaine non sécurisés. 
- **L'attaque vous permet de passer administrateur si vous dumpez le ticket d'un administrateur de domaine, puis que vouz empruntez l'identité de ce ticket à l'aide d'une attaque PTT mimikatz, vous permettant ainsi d'agir en tant qu'administrateur**. 
- Vous pouvez penser à une attaque par "laissez-passer" comme si la réutilisation d'un ticket existant ne créait ou ne détruisait aucun ticket ici, mais réutilisait simplement un ticket existant d'un autre utilisateur du domaine et usurpait l'identité de ce ticket.

<img src="https://imgur.com/V6SOlll.png">

---
### Préparez votre attaque

**Préparez les tickets Mimikatz & Dump** 

Préalablement, Essayez d'accéder au partage administrateur \\IP\admin
Que constatez-vous ?
```
réponse
```

Vous devrez exécuter l'invite de commande en tant qu'administrateur : utilisez les mêmes informations d'identification que vous pour accéder à la machine. Si vous n'avez pas d'invite de commande élevée, mimikatz ne fonctionnera pas correctement.

1.) ```cd Downloads``` - accédez au répertoire dans lequel se trouve mimikatz

2.) ```mimikatz.exe``` - exécutez mimikatz

3.) ```privilege::debug``` - Assurez-vous que cela renvoie [output '20' OK] si ce n'est pas le cas, cela signifie que vous n'avez pas les privilèges d'administrateur pour exécuter correctement mimikatz

<img src="https://imgur.com/SJQGplV.png">

4.) ```sekurlsa::tickets /export``` - ceci exportera tous les tickets .kirbi dans le répertoire dans lequel vous vous trouvez actuellement

A cette étape, vous pouvez également utiliser les tickets encodés en base 64 de Rubeus que nous avons récoltés plus tôt

- Lorsque vous recherchez un ticket à "emprunter", il est recommandé de rechercher un ticket administrateur à partir du krbtgt, tout comme celui décrit en rouge ci-dessous.

<img src="https://imgur.com/xC0L5Kf.png">

**Passer le ticket avec Mimikatz**

Maintenant que notre ticket est prêt, nous pouvons maintenant effectuer une attaque de type « pass the ticket » pour obtenir les privilèges d'administrateur de domaine.

1.) ```kerberos::ptt <ticket>``` - exécutez cette commande dans mimikatz avec le ticket que vous avez récupéré plus tôt. Il mettra en cache et usurpera l'identité du ticket donné

<img src="https://imgur.com/DwXmm8Z">

2.) ```klist``` - Ici, nous vérifions simplement que nous avons usurpé avec succès l'identité du ticket en répertoriant nos tickets mis en cache.

<img src="https://imgur.com/GgxDm9k.png">

Question : Quel effet a eu la commande mimkatz ? Que s'est il réellement concrètement ?
```
réponse
```

Nous n'utiliserons pas mimikatz pour le reste de l'attaque.


3.) Vous avez maintenant usurpé l'identité du ticket vous donnant les mêmes droits que le TGT que vous usurpez l'identité. Pour vérifier cela, nous pouvons regarder le partage admin.

Essayez d'accéder au partage administrateur
Que constatez-vous ?
```
réponse
```

<img src="https://imgur.com/9nxjeTS.png">

Notez qu'il ne s'agit que d'un POC pour comprendre comment passer le ticket et gagner l'administrateur du domaine.  
La façon dont vous abordez le passage du ticket peut être différente en fonction du type d'engagement dans lequel vous vous trouvez pour lancer cette attaque.

---

**Mitigation du Passage de tickets** -

Parlons Blue Team et comment atténuer ces types d'attaques :

Ne laissez pas vos administrateurs de domaine se connecter à autre chose que le contrôleur de domaine -
C'est quelque chose de si simple, mais de nombreux administrateurs de domaine se connectent toujours à des ordinateurs de bas niveau en laissant des tickets qui peuvent être utilisés pour attaquer et se déplacer latéralement.


----
  
### Golden Ticket avec Mimikatz
  
Mimikatz est un outil de post-exploitation très populaire et puissant le plus couramment utilisé pour vider les informations d'identification des utilisateurs à l'intérieur d'un réseau Active Directory, mais vous pouvez également utiliser mimikatz afin de créer un Silver ticket.

Un Silver Ticket peut parfois être mieux utilisé dans les fiançailles plutôt qu'un Golden Ticket car il est un peu plus discret. Si la furtivité et le fait de ne pas être détecté sont importants, un Silver Ticket est probablement une meilleure option qu'un Golden Ticket, mais l'approche pour en créer un est exactement la même. La principale différence entre les deux tickets est qu'un ticket silver est limité au service ciblé alors qu'un ticket golden a accès à n'importe quel service Kerberos.

Un scénario d'utilisation spécifique pour un Silver Ticket serait que vous souhaitiez accéder au serveur SQL du domaine, mais votre utilisateur compromis actuel n'a pas accès à ce serveur. Vous pouvez trouver un compte de service accessible pour prendre pied en kerberoasting ce service, vous pouvez ensuite dumper le hachage du service, puis emprunter l'identité de leur TGT afin de demander un ticket de service pour le service SQL du KDC vous permettant d'accéder au SQL du domaine serveur.

Présentation de KRBTGT

Afin de bien comprendre le fonctionnement de ces attaques, vous devez comprendre quelle est la différence entre un KRBTGT et un TGT. Un KRBTGT est le compte de service du KDC. Il s'agit du centre de distribution de clés qui délivre tous les tickets aux clients. Si vous usurpez l'identité de ce compte et créez un Golden Ticket du KRBTGT, vous vous donnez la possibilité de créer un ticket de service pour tout ce que vous voulez. Un TGT est un ticket pour un compte de service émis par le KDC et ne peut accéder qu'à ce service dont le TGT provient, comme le ticket SQLService.

**Aperçu de l'attaque par Golden/Silver ticket -**

Une attaque par Golden Ticket fonctionne en dumpant le ticket d'octroi de ticket de n'importe quel utilisateur sur le domaine, ce serait de préférence un administrateur de domaine, mais pour un Golden Ticket, vous videriez le ticket krbtgt et pour un Silver Ticket, vous dumperiez n'importe quel service ou administrateur de domaine ticket. Cela vous fournira le SID ou l'identifiant de sécurité du compte d'administrateur de service/domaine qui est un identifiant unique pour chaque compte d'utilisateur, ainsi que le hachage NTLM. Vous utilisez ensuite ces détails dans une attaque par Golden Ticket mimikatz afin de créer un TGT qui usurpe l'identité des informations de compte de service données.



dumpez le hachage krbtgt -

1.) ```cd downloads && mimikatz.exe``` - accédez au répertoire dans lequel se trouve mimikatz et exécutez mimikatz

2.) ```privilege::debug``` - assurez-vous que ces sorties [privilege '20' ok]

3.) lsadump::lsa /inject /name:krbtgt - Cela dumpera le hachage ainsi que l'identifiant de sécurité nécessaire pour créer un Golden Ticket. Pour créer un Silver Ticket, vous devez modifier le /name : pour vider le hachage d'un compte d'administrateur de domaine ou d'un compte de service tel que le compte SQLService.



**Créer un ticket golden/silver -**

1.) ```Kerberos::golden /user:Administrator /domain:controller.local /sid: /krbtgt: /id:``` 
  C'est la commande pour créer un Golden ticket. 
  Pour créer un Silver ticket il suffit de mettre un hachage NTLM de service dans le krbtgt slot, le sid du compte de service en sid, et changez l'id en 1103.

Je vais vous montrer une démo de création d'un Golden Ticket c'est à vous de créer un Silver Ticket.


Utilisez le Golden/Silver Ticket pour accéder à d'autres machines -

1.) ```misc::cmd``` - cela ouvrira une nouvelle invite de commande élevée avec le ticket donné dans mimikatz.


2.) Accédez aux machines que vous voulez, ce à quoi vous pouvez accéder dépendra des privilèges de l'utilisateur à qui vous avez décidé de prendre le ticket.   
Cependant, si vous avez pris le ticket de krbtgt, vous avez accès à TOUT le réseau d'où le nom golden ticket.
Toutefois, les Silver Ticket n'ont accès qu'à ceux auxquels l'utilisateur a accès s'il s'agit d'un administrateur de domaine, il peut presque accéder à l'ensemble du réseau, mais il est légèrement moins élevé qu'un Golden Ticket.


Cette attaque ne fonctionnera pas sans d'autres machines sur le domaine, mais je vous mets au défi de la configurer sur votre propre réseau et d'essayer ces attaques.

**Répondre aux questions ci-dessous**
  
Quel est le hash du SQLService NTLM ?
```
reponse : 
```
  
Quel est le hash du Administrateur NTLM ?
```
reponse : 
```

  
