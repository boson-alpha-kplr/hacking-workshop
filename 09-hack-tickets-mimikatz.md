## Passage de Ticket Kerberos avec Mimikatz

Mimikatz est un outil de post-exploitation très populaire et puissant le plus couramment utilisé pour dumper les informations d'identification des utilisateurs à l'intérieur d'un réseau Active Directory. 

**Dans le présent worskshop, nous allons utiliser mimikatz pour dumper un TGT depuis la mémoire LSASS.**

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
Que constatez-vous ? Pourquoi ?
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

<img src="https://imgur.com/DwXmm8Z.png">

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
