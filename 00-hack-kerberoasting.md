---
### KERBEROASTING ("Rotissâge")

<img src="https://www.shopenseigne.fr/wp-content/uploads/2020/05/enseigne-lumineuse-double-face-rotisserie-ref-06-shop-enseigne-production-13001-marseille-4.jpg" width="30%" height="30%">

- Dans cette phase, nous couvrirons l'une des attaques Kerberos les plus populaires : Le Kerberoasting.  
- Le Kerberoasting permet à un utilisateur de demander un ticket de service pour tout service avec un SPN enregistré, puis d'utiliser ce ticket pour déchiffrer le mot de passe du service.   
- Si le service a un SPN enregistré, il peut être Kerberoastable, mais le succès de l'attaque dépend de la force du mot de passe et s'il est traçable ainsi que des privilèges du compte de service craqué.  
- Pour énumérer les comptes Kerberoastable, je suggérerais **un outil comme BloodHound** pour trouver tous les comptes "Kerberoastables", il vous permettra de voir quel type de comptes vous pouvez kerberoast s'ils sont administrateurs de domaine, et quel type de connexions ils ont avec le reste du domaine.  
C'est un excellent outil pour trouver des comptes à cibler. 
- Si vous le souhaitez vous pouvez l'exécuter pour explorer le domaine dans un premier temps

Afin d'effectuer l'attaque, nous utiliserons à la fois Rubeus et Impacket afin que vous compreniez les différents outils disponibles pour Kerberoasting. 
Il existe d'autres outils tels que kekeo et Invoke-Kerberoast, mais je vous laisse faire vos propres recherches sur ces outils.

Pour vous faciliter la tâche, **Rubeus est déjà mis sur la machine pour vous**. 
il se trouve dans le dossier de téléchargements.


### **Kerberoasting avec Rubeus** 

1.) Téléchargements de cd - accédez au répertoire dans lequel se trouve Rubeus

2.) Rubeus.exe kerberoast Cela videra le hachage Kerberos de tous les utilisateurs kerberoastable

<img src="https://imgur.com/XZegVqf.png"/>

copiez le hash sur votre machine attaquante et placez-le dans un fichier .txt afin que nous puissions le cracker avec hashcat

Nous utiliserons une liste de mots rockyou modifiée afin d'accélérer le processus, téléchargez-la ici :
https://raw.githubusercontent.com/Cryilllic/Active-Directory-Wordlists/master/Pass.txt

3.) ```hashcat -m 13100 -a 0 hash.txt Pass.txt``` - maintenant cracker ce hash


### **Méthode 2 - Impacket**

**Installation d'Impacket**

Les versions d'Impacket sont instables depuis la 0.9.20. Je suggère d'avoir une installation d'Impacket < 0.9.20

1.) cd /opt accédez à votre répertoire préféré pour enregistrer les outils dans

2.) téléchargez le package précompilé à partir de ```https://github.com/SecureAuthCorp/impacket/releases/tag/impacket_0_9_19```

3.) cd Impacket-0.9.19 accédez au répertoire impacket

4.) pip installer. - cela installera toutes les dépendances nécessaires

**Kerberoasting avec Impacket**

1.) ```cd /usr/share/doc/python3-impacket/examples/ - accédez à l'emplacement de GetUserSPNs.py```

2.) ```sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip MACHINE_IP -request``` - cela videra le hachage Kerberos pour tous les comptes kerberoastable qu'il peut trouver sur le domaine cible, comme le fait Rubeus. 
Cependant, cela ne doit pas nécessairement être sur la machine cible et peut être effectué à distance.

3.) ```hashcat -m 13100 -a 0 hash.txt Pass.txt``` -  cracker ce hachage

**Que peut faire un compte de service ?**

- Après avoir déchiffré le mot de passe du compte de service, il existe différentes manières d'exfiltrer des données ou de collecter du butin selon que le compte de service est un administrateur de domaine ou non.  
- Si le compte de service est un administrateur de domaine, vous disposez d'un contrôle similaire à celui d'un ticket doré/argent et pouvez désormais collecter du butin tel que le dumping du NTDS.dit.  
- Si le compte de service n'est pas un administrateur de domaine, vous pouvez l'utiliser pour vous connecter à d'autres systèmes et faire pivoter ou escalader ou vous pouvez utiliser ce mot de passe piraté pour pulvériser contre d'autres comptes d'administrateur de service et de domaine
- De nombreuses entreprises peuvent réutiliser des mots de passe identiques ou similaires pour leurs utilisateurs administrateurs de service ou de domaine.   
- Si vous participez à un test d'intrusion professionnel, sachez comment l'entreprise veut que vous montriez le risque la plupart du temps, elle ne veut pas que vous exfiltrez des données et vous fixera un objectif ou un processus à atteindre afin de montrer le risque à l'intérieur de l'évaluation.

**Mitigation : Défendre la forêt**

Se prémunir du Kerberoasting :

- Opter pour Mots de passe de service forts - Si les mots de passe du compte de service sont forts, le kerberoasting sera inefficace
- N'autorisez pas de comptes de service comme administrateurs de domaine - Les comptes de service n'ont pas besoin d'être des administrateurs de domaine, kerberoasting ne sera pas aussi efficace si vous ne faites pas des comptes de service comme administrateurs de domaine.

**Répondre aux questions ci-dessous**

Qu'est-ce que le mot de passe HTTPService ?

Qu'est-ce que le mot de passe SQLService ?

---

### Roasting AES

Très similaire à Kerberoasting, AS-REP Roasting vide les hachages krbasrep5 des comptes d'utilisateurs pour lesquels la pré-authentification Kerberos est désactivée. Contrairement à Kerberoasting, ces utilisateurs n'ont pas besoin d'être des comptes de service, la seule exigence pour pouvoir rôtir AS-REP un utilisateur est que l'utilisateur doit avoir désactivé la pré-authentification.

Nous continuerons à utiliser Rubeus comme nous l'avons fait avec le kerberoasting et la récolte, car Rubeus a une commande très simple et facile à comprendre pour rôtir AS-REP et attaquer les utilisateurs avec la pré-authentification Kerberos désactivée. Après avoir vidé le hachage de Rubeus, nous utiliserons hashcat afin de casser le hachage krbasrep5.

Il existe également d'autres outils pour la torréfaction AS-REP tels que kekeo et GetNPUsers.py d'Imppacket. Rubeus est plus facile à utiliser car il trouve automatiquement les utilisateurs AS-REP Roastable alors qu'avec GetNPUsers, vous devez énumérer les utilisateurs au préalable et savoir quels utilisateurs peuvent être AS-REP Roastable.

J'ai déjà compilé et mis Rubeus sur la machine.

**Aperçu du Roasting AS-REP** -

Lors de la pré-authentification, le hachage des utilisateurs sera utilisé pour chiffrer un horodatage que le contrôleur de domaine tentera de déchiffrer pour valider que le bon hachage est utilisé et ne relit pas une demande précédente. Après avoir validé l'horodatage, le KDC émettra un TGT pour l'utilisateur. Si la pré-authentification est désactivée, vous pouvez demander des données d'authentification pour n'importe quel utilisateur et le KDC renverra un TGT crypté qui peut être craqué hors ligne car le KDC saute l'étape de validation que l'utilisateur est vraiment ce qu'il prétend être.

**Vidange des hachages KRBASREP5 avec Rubeus** -

1.) cd Downloads- accédez au répertoire dans lequel se trouve Rubeus

2.) Rubeus.exe asreprooast - Cela exécutera la commande AS-REP rôti à la recherche d'utilisateurs vulnérables, puis videra les hachages d'utilisateurs vulnérables trouvés.

**Crack des hashes avec hashcat** -

1.) Transférez le hachage de la machine cible vers votre machine attaquante et placez le hachage dans un fichier txt

2.) Insérez 23$ après $krb5asrep$ pour que la première ligne soit $krb5asrep$23$User.....

Utilisez la même liste de mots que vous avez téléchargée dans la tâche 4

3.) ```hashcat -m 18200 hash.txt Pass.txt``` - crackez ces hachages ! Rubeus AS-REP Roasting utilise le mode hashcat 18200


**Mitigation de roasting AS-REP**

Ayez une politique de mot de passe fort. Avec un mot de passe fort, les hachages mettront plus de temps à se fissurer, rendant cette attaque moins efficace
Ne désactivez pas la pré-authentification Kerberos, sauf si cela est nécessaire, il n'y a presque pas d'autre moyen d'atténuer complètement cette attaque que de maintenir la pré-authentification activée.

**Répondre aux questions ci-dessous**
Quel type de hachage utilise AS-REP Roasting ?

Quel utilisateur est vulnérable au Roasting AS-REP ?

Quel est le mot de passe de l'utilisateur ?

Quel administrateur est vulnérable au Roasting AS-REP ?

Quel est le mot de passe de l'administrateur ?

---
