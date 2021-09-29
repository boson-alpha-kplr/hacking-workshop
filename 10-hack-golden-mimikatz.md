----
  
### Golden/Silver Ticket avec Mimikatz
  
Cette fois nous allons utiliser mimikatz afin de créer dans un premier temps **un Silver ticket**.

- Un Silver Ticket peut parfois être mieux utilisé dans les engagements de pentest plutôt qu'un Golden Ticket car il est un peu plus discret.  
- Si la furtivité et le fait de ne pas être détecté sont importants, un Silver Ticket est probablement une meilleure option qu'un Golden Ticket, mais l'approche pour en créer un est exactement la même. 
- **La principale différence entre les deux tickets est qu'un ticket silver est limité au service ciblé alors qu'un ticket golden a accès à n'importe quel service Kerberos**.

**Exemple de Scénario**

- Un scénario d'utilisation spécifique pour un Silver Ticket serait que vous souhaitiez accéder au serveur SQL du domaine, mais votre utilisateur compromis actuel n'a pas accès à ce serveur.   
- Vous pouvez trouver un compte de service accessible pour tenter le kerberoasting de ce service, vous pouvez ensuite dumper le hachage du service, puis emprunter l'identité de leur TGT afin de demander un ticket de service pour le service SQL du KDC vous permettant d'accéder au serveur SQL du domaine.

**Présentation de KRBTGT**

- Afin de bien comprendre le fonctionnement de ces attaques, vous devez comprendre quelle est la différence entre un KRBTGT et un TGT.    
- **Un KRBTGT est le compte de service du KDC.** Il s'agit du centre de distribution de clés qui délivre tous les tickets aux clients. Si vous usurpez l'identité de ce compte et créez un Golden Ticket du KRBTGT, vous vous donnez la possibilité de créer un ticket de service pour tout ce que vous voulez !!!  
-  Un TGT est en revanche un ticket pour un compte de service émis par le KDC et ne peut accéder qu'à ce service dont le TGT provient(comme le ticket SQLService).

**Aperçu de l'attaque par Golden/Silver ticket -**

Une attaque par Golden/Silver Ticket fonctionne en dumpant le ticket d'octroi de ticket (TGT) de n'importe quel utilisateur sur le domaine, (celui d'un administrateur de domaine est toujours préférable)  
- Pour un Golden Ticket, **vous dumperiez le ticket krbtgt** 
- pour un Silver Ticket, **vous dumperiez le ticket n'importe quel service ou administrateur de domaine**.
- Cela vous fournira le SID ou l'identifiant de sécurité du compte d'administrateur de service/domaine qui est un identifiant unique pour chaque compte d'utilisateur, ainsi que le hachage NTLM.  
- Vous utilisez ensuite ces détails dans une attaque par Golden Ticket mimikatz afin de créer un TGT qui usurpe l'identité des informations de compte de service données.

---

**Procédons à l'attaque**

## dumpez le hash du krbtgt

1.) ```cd downloads && mimikatz.exe``` - accédez au répertoire dans lequel se trouve mimikatz et exécutez mimikatz

2.) ```privilege::debug``` - assurez-vous que ces sorties [privilege '20' ok]

3.) ```lsadump::lsa /inject /name:krbtgt``` - Cela dumpera le hachage ainsi que l'identifiant de sécurité nécessaire pour créer un Golden Ticket. Pour créer un Silver Ticket, vous devez modifier le /name : pour vider le hachage d'un compte d'administrateur de domaine ou d'un compte de service tel que le compte SQLService.

<img src="https://imgur.com/BSh4rXy.png"/>

## Créer un ticket golden/silver 

1.) ```Kerberos::golden /user:Administrator /domain:controller.local /sid: /krbtgt: /id:``` 
  C'est la commande pour créer un Golden ticket. 
  Pour créer un Silver ticket il suffit de mettre un hash NTLM de service dans le krbtgt slot, le sid du compte de service en sid, et changez l'id en 1103.

Ci dessous vous voyez la création d'un Golden Ticket. 
A vous de créer un Silver Ticket !

<img src="https://imgur.com/6HnEnwi.png"/>

## Utilisez le Golden/Silver Ticket pour accéder à d'autres machines

1.) ```misc::cmd``` - cela ouvrira une nouvelle invite de commande élevée avec le ticket donné dans mimikatz.

<img src="https://imgur.com/rh06qDl.png"/>


2.) Accédez aux machines que vous voulez, ce à quoi vous pouvez accéder dépendra des privilèges de l'utilisateur à qui vous avez décidé de prendre le ticket.   
- si vous avez pris le ticket de krbtgt, vous avez accès à TOUT le réseau d'où le nom golden ticket.  
- les Silver Ticket n'ont accès qu'à ceux auxquels l'utilisateur a accès s'il s'agit d'un administrateur de domaine, il peut presque accéder à l'ensemble du réseau, mais il est légèrement moins élevé qu'un Golden Ticket.

<img src="https://imgur.com/VOEsU4O.png"/>

Cette attaque ne fonctionnera pas sans d'autres machines sur le domaine, mais vous pouvez tenter de la configurer sur votre propre réseau local et d'essayer ces attaques.

**Répondre aux questions ci-dessous**
  
Quel est le hash du SQLService NTLM ?
```
reponse : 
```
  
Quel est le hash du Administrateur NTLM ?
```
reponse : 
```

  
