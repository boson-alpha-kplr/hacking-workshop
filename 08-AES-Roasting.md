### Roasting AES

- Très similaire au Kerberoasting, AS-REP Roasting dump les hachages krbasrep5 des comptes d'utilisateurs pour lesquels la pré-authentification Kerberos est désactivée.   

- Contrairement à Kerberoasting, ces utilisateurs n'ont pas besoin d'être des comptes de service, la seule exigence pour pouvoir griller AS-REP un utilisateur est que l'utilisateur doit avoir désactivé la pré-authentification.

- **Lecture Utile Préalable** : https://thehackernews.com/2021/09/what-is-as-rep-roasting-attack-really.html

- L'une des premières étapes de l'authentification Kerberos est **la pré-authentification**. La pré-authentification utilise le mot de passe de l'utilisateur pour chiffrer un horodatage (timestamp).   
- Le contrôleur de domaine (DC) déchiffrera cela pour valider le mot de passe correct et ne pas rejouer une demande précédente (**replay attack**). 
- <ins>Une vulnérabilité peut donc survenir lorsque la pré-authentification est désactivée.</ins>

- Une fois cette option désactivée, un pirate peut demander des données d'authentification pour n'importe quel utilisateur, et le contrôleur de domaine renverra un ticket d'octroi de ticket (TGT) crypté.  
-  Il peut ensuite être utilisé pour forcer brutalement dans un environnement hors ligne pour déchiffrer le mot de passe.

### Outillage 🛠️ :

- Nous continuerons à utiliser Rubeus comme nous l'avons fait avec le kerberoasting et le harvesting, car Rubeus a une commande très simple et facile à comprendre pour roaster AS-REP et attaquer les utilisateurs avec la pré-authentification Kerberos désactivée.  
-
- Après avoir dumpé le hash de Rubeus, nous utiliserons hashcat afin de casser le hachage krbasrep5.

- Il existe également d'autres outils pour le roasting AS-REP tels que kekeo et GetNPUsers.py d'Imppacket.  
- Rubeus est plus facile à utiliser car il trouve automatiquement les utilisateurs AS-REP Roastable alors qu'avec GetNPUsers, vous devez énumérer les utilisateurs au préalable et savoir quels utilisateurs peuvent être "AS-REP Roastable".

Rubeus est déjà copilé sur la machine cible 🙌

---

**Résumé du Roasting AS-REP** -

- Lors de la pré-authentification, le hachage des utilisateurs sera utilisé pour chiffrer un horodatage que le contrôleur de domaine tentera de déchiffrer pour valider que le bon hachage est utilisé et ne relit pas une demande précédente.  
- 
- Après avoir validé l'horodatage, le KDC émettra un TGT pour l'utilisateur.  
- **_Si la pré-authentification est désactivée, vous pouvez demander des données d'authentification pour n'importe quel utilisateur et le KDC renverra un TGT crypté qui peut être craqué offline_** car le KDC saute l'étape de validation dufait que "l'utilisateur est vraiment ce qu'il prétend être".

---

**Dumping des hash KRBASREP5 avec Rubeus** -

1.) ```cd Downloads```- accédez au répertoire dans lequel se trouve Rubeus

2.) ```Rubeus.exe asreproast``` - Cela exécutera la commande AS-REP roasté à la recherche d'utilisateurs vulnérables, puis dumpera les hachages d'utilisateurs vulnérables trouvés.

**Crack des hashes avec hashcat** -

1.) Transférez le hachage de la machine cible vers votre machine attaquante et placez le hachage dans un fichier txt

2.) Insérez 23$ après $krb5asrep$ pour que la première ligne soit $krb5asrep$23$User.....

Utilisez la même liste de mots que vous avez téléchargée dans la tâche 4

3.) ```hashcat -x nnnn aaaa bbbb``` - crackez ces hachages ! 
Hint : trouvez le mode hashcat qui convient pour ce type de roasting AS-REP (5 chiffres)
Si vous êtes coincés demandez-le au formateur.

**Mitigation de roasting AS-REP**

Ayez une politique de mot de passe fort. Avec un mot de passe fort, les hachages mettront plus de temps à se fissurer, rendant cette attaque moins efficace
Ne désactivez pas la pré-authentification Kerberos, sauf si cela est nécessaire, il n'y a presque pas d'autre moyen d'atténuer complètement cette attaque que de maintenir la pré-authentification activée.

**Répondre aux questions ci-dessous**


Quel type de hachage utilise AS-REP Roasting ?
```
reponse
```
qu'est ce que KRBASREP5 ?
```
reponse
```
Quel utilisateur est vulnérable au Roasting AS-REP ?
```
reponse
```
Quel est le mot de passe de l'utilisateur ?
```
reponse
```
Quel administrateur est vulnérable au Roasting AS-REP ?
```
reponse
```
Quel est le mot de passe de l'administrateur ?
```
reponse
```
Quel est le mot de passe de l'administrateur ?
```
reponse
```
Quelle est la différence de cette attaque par rapport aux précédentes ?
```
reponse
```
Dans quel cas peut-il être avantageux de l'utiliser par rapport aux autres ?
```
reponse
```
---

**BONUS :** Si vous avez terminé rapidement, tentez la même attaque avec Impacket
