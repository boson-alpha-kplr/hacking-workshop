# Creation de Backdoor Kerberos avec Mimikatz

En plus de maintenir l'accès à l'aide de Golden/Silver tickets, mimikatz a un autre atout dans ses manches lorsqu'il s'agit d'attaquer Kerberos.  

Contrairement aux attaques par ticket classiques, un backdoor Kerberos est beaucoup plus subtile car il agit de la même manière qu'un rootkit en s'implantant dans la mémoire de la forêt de domaines, ce qui lui permet d'accéder à n'importe laquelle des machines avec un mot de passe principal.

Le Backdoor Kerberos fonctionne en implantant une clé squelette (_skeleton key_) qui abuse de la façon dont l'AS-REQ valide les horodatages chiffrés. Une clé squelette ne fonctionne qu'avec le cryptage Kerberos RC4.

Le hachage par défaut pour une clé squelette mimikatz est 60BA4FCADC466C7A033C178194C03DF6, ce qui rend le mot de passe -"mimikatz"

Ce workshop ne sera qu'une section de présentation et ne vous obligera pas à faire quoi que ce soit sur la machine, mais je vous encourage à continuer par vous-même ultérieurement et à ajouter d'autres machines et à tester en utilisant des clés squelettes avec mimikatz.

**Fonctionnement de la clé squelette** -

- La clé squelette fonctionne en abusant des horodatages chiffrés AS-REQ.  
comme expliqué ci-dessus, l'horodatage est chiffré avec le hachage NT des utilisateurs.  
- Le contrôleur de domaine essaie ensuite de déchiffrer cet horodatage avec le hachage NT des utilisateurs, une fois qu'une clé squelette est implantée, le contrôleur de domaine essaie de déchiffrer l'horodatage en utilisant à la fois le hachage NT de l'utilisateur et le hachage NT de la clé squelette vous permettant d'accéder à la forêt du domaine.



**Préparation de Mimikatz** -

1.) ```cd Downloads && mimikatz.exe``` - Accédez au répertoire dans lequel se trouve mimikatz et exécutez mimikatz

2.) ```privilege::debug``` - Cela devrait être une norme pour l'exécution de mimikatz car mimikatz a besoin d'un accès administrateur local



**Installation de la clé squelette avec mimikatz **

1.) ```misc::skeleton``` - Oui ! c'est tout, mais ne sous-estimez pas cette petite commande elle est très puissante ☠️



**Accéder à la forêt** 

Les identifiants par défaut seront : "mimikatz"

exemple : ```net use c:\\DOMAIN-CONTROLLER\admin$ /user:Administrator mimikatz``` - Le partage sera désormais accessible sans avoir besoin du mot de passe Administrateurs

exemple : ```dir \\Desktop-1\c$ /user:Machine1 mimikatz``` - accéder au répertoire de Desktop-1 sans jamais savoir quels utilisateurs ont accès à Desktop-1

La clé squelette ne persistera pas par elle-même car elle s'exécute dans la mémoire, elle peut être scriptée ou persistée à l'aide d'autres outils et techniques, mais cela est hors de portée pour ce worskshop.

**Répondez aux questions** 

Qu'est ce qui rend une telle attaque possible ?
```
reponse
```

Comment pourrait-on s'en protéger et l'empêcher ?
```
reponse
```
