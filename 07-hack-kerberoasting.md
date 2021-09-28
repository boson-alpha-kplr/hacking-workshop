### KERBEROASTING ("Rotissâge")

<img src="https://www.shopenseigne.fr/wp-content/uploads/2020/05/enseigne-lumineuse-double-face-rotisserie-ref-06-shop-enseigne-production-13001-marseille-4.jpg" width="30%" height="30%">

- Dans cette phase, nous couvrirons l'une des attaques Kerberos les plus populaires : Le Kerberoasting. 

- Le Kerberoasting est une attaque post-exploitation qui extrait les hash d'informations d'identification de compte de service d'Active Directory pour les craquer en offline. 

- C'est une attaque courante et omniprésente qui exploite une combinaison de chiffrement faible et de mauvaise hygiène des mots de passe des comptes de service.

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

1.) ```cd downloads``` - accédez au répertoire dans lequel se trouve Rubeus

2.) ```Rubeus.exe kerberoast``` -  Cela videra le hachage Kerberos de tous les utilisateurs kerberoastable

<img src="https://imgur.com/XZegVqf.png"/>

copiez le hash sur votre machine attaquante et placez-le dans un fichier .txt afin que nous puissions le cracker avec hashcat

Nous utiliserons une liste de mots rockyou modifiée afin d'accélérer le processus, téléchargez-la ici :
https://raw.githubusercontent.com/Cryilllic/Active-Directory-Wordlists/master/Pass.txt

3.) ```hashcat -m 13100 -a 0 hash.txt Pass.txt``` - maintenant cracker ce hash


### **Méthode 2 - Impacket**

**Installation d'Impacket**

Les versions d'Impacket sont instables depuis la 0.9.20. Je suggère d'avoir une installation d'Impacket < 0.9.20

1.) ```cd /opt``` -  accédez à votre répertoire préféré pour enregistrer les outils dans

2.) téléchargez le package précompilé à partir de ```https://github.com/SecureAuthCorp/impacket/releases/tag/impacket_0_9_19```

3.) ```cd Impacket-0.9.1``` - accédez au répertoire impacket

4.) ```pip install requirements.txt``` - cela installera toutes les dépendances nécessaires

**Kerberoasting avec Impacket**

1.) ```cd /usr/share/doc/python3-impacket/examples/ - accédez à l'emplacement de GetUserSPNs.py```

2.) ```sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip MACHINE_IP -request``` - cela videra le hachage Kerberos pour tous les comptes kerberoastable qu'il peut trouver sur le domaine cible, comme le fait Rubeus.   
Cependant, cela ne doit pas nécessairement être sur la machine cible et peut être effectué à distance.

3.) ```hashcat -m 13100 -a 0 hash.txt Pass.txt``` -  cracker ce hachage

**Que peut faire un compte de service ?**

- Après avoir déchiffré le mot de passe du compte de service, il existe différentes manières d'exfiltrer des données ou de collecter un butin selon que le compte de service est un administrateur de domaine ou non.  
- Si le compte de service est un administrateur de domaine, vous disposez d'un contrôle similaire à celui d'un ticket Golden/Silver et pouvez désormais collecter un butin tel que le dumping du NTDS.dit.  
- Si le compte de service n'est pas un administrateur de domaine, vous pouvez l'utiliser pour vous connecter à d'autres systèmes et faire pivoter ou escalader ou vous pouvez utiliser ce mot de passe piraté pour pulvériser contre d'autres comptes d'administrateur de service et de domaine
- De nombreuses entreprises peuvent réutiliser des mots de passe identiques ou similaires pour leurs utilisateurs administrateurs de service ou de domaine.   
- Si vous participez à un test d'intrusion professionnel, sachez comment l'entreprise veut que vous montriez le risque la plupart du temps, elle ne veut pas que vous exfiltrez des données et vous fixera un objectif ou un processus à atteindre afin de montrer le risque à l'intérieur de l'évaluation.

---

### **Mitigation : Défendre la forêt**

**Se prémunir du Kerberoasting :**

- :white_check_mark: Optez pour des mots de passe de service forts :muscle: - Si les mots de passe du compte de service sont forts, le kerberoasting sera inefficace
- :no_entry_sign: N'autorisez pas de comptes de service comme administrateurs de domaine :no_entry_sign: :   
  Les comptes de service n'ont pas besoin d'être des administrateurs de domaine, kerberoasting ne sera pas aussi efficace si vous ne faites pas des comptes de service comme administrateurs de domaine.

**Répondre aux questions ci-dessous**

Quel est le mot de passe HTTPService ?
```
reponse
```
Quel est le mot de passe SQLService ?
```
reponse
```

