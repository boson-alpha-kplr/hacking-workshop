6 : Quels ports serait-il préférable d’utiliser dans le cas de la mise en place d’une connexion de type reverse, passant au travers un pare-feu d’entreprise ? (3 réponses obligatoires)

8 : Quels sont les protocoles utilisés dans le cas d’une authentification au sein d’un domaine Microsoft Windows ? (2 réponses obligatoires)

9 : Quels sont les prérequis pour le bon fonctionnement d’un proxy "socks4a" ? (2 réponses obligatoires)

10 : Quelles mesures de sécurité faudrait-il appliquer pour empêcher une attaque de type "Man In The Middle" sur le protocole RDP ? (2 réponses obligatoires)

11 : Quel compte permet de générer des TGT valides ?

12 : Quelle est la suite logique de ces protocoles de résolution de noms ?

By default, Windows systems use the following prioritized list of services to attempt to resolve name resolution request:
`Domain Name Service (DNS)`
`Link-Local Multicast Name Resolution (LLMNR)`
`NetBIOS Name Service (NBNS/WINS)`

13 : Quels sont les prérequis pour forger un Golden Ticket ? (2 réponses obligatoires)


14 : Dans le cas d’une authentification Kerberos au sein d’un domaine Microsoft Windows, à l’issue des différentes phases de communications avec l’AS (Authentication Service), quels sont les 2 éléments qui sont envoyés au client ? (2 réponses obligatoires)

15 : Quel système de protection permet de limiter au maximum la modification des ACLs des groupes "Admins du domaine" et "Administrateurs du schéma" ?

16 : Quel outil permet la synchronisation avec les éléments d’un contrôleur de domaine Active Directory ?

18 : Quel privilège permet de réaliser un dump de processus ?

19 : Que signifie l’acronyme "SSP" ?

20 : Dans un fichier Excel, quelles techniques permettent l'exécution de code à l’ouverture de celui-ci ? (2 réponses obligatoires)
`DDE / MACRO`

21 : Quelles techniques sont utilisées pour réaliser des mouvements latéraux ? (3 réponses obligatoires)
`PTH / PTT / Golden Ticket`

22 : Quelle commande faut-il utiliser pour créer une charge malveillante pour Metasploit ?
`msfvenom`

23 : Quel élément est un hash de type "hash-NT" ?
`d72482ccabdc2b17863e90d468cec4d0`

24 : Quelle charge est de type "stageless" ?
`windows/meterpreter_reverse_tcp`
https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/

25 : Quelle commande permet d’afficher les membres du groupe "Admins du domaine" ?
`net group /Domain "Admins du domaine"`


26 : Quelle commande permet de définir une option dans Metasploit ?
`set`

27 : Quelles mesures de sécurité permettent d'empêcher, au maximum, les attaques de type "phishing" ? (3 réponses obligatoires)
https://cipher.com/blog/phishing-protection-spf-dkim-dmarc/
`DKIM / DMARC / SPF`


28 : Quelle est la durée de vie d’un ticket TGT ?
`10 minutes` (?)

29 : Quelle commande de Mimikatz permet de rentrer en mode "debug" ?
`debug::privilege`

30 : Quel élément n’est pas un SSP ?
`LiveSSP`

31 : Quels outils peuvent permettre à un administrateur local sur un serveur, de prendre le contrôle d’une session utilisateur logué sur ce même serveur, sans devoir saisir son mot de passe ? (2 réponses obligatoires)
`MMC / PsExec`

32 :  deskNTLMV2 Attaccks
`Pass the hash / Relay`

33 : Dans Metasploit, quel est le nom du module d’écoute permettant de recevoir les connexions Meterpreter ?
`Listener`

34 : La mise en place d’un "pivoting" dans le cadre d’un test d’intrusion peut être pertinent pour effectuer quelles actions ? (3 réponses obligatoires)

35 : Quelle vulnérabilité est connue sous le nom de "EternalBlue" ?
`MS17`-010

36 : Quelle option permet d'empêcher une attaque par relais suite à l’obtention d’un hash NTLMv2 ?
`Communications signées numériquement`

37 : Dans quel processus sont stockés les hashs NTLM destinés aux SSP ?
`lsass`

38 : Quel compte est utilisé pour générer des tickets Kerberos ?
`Krbtgt`

39 : Avec quels types de hashs est-il possible de réaliser un PTH ? (2 réponses obligatoires)
`NTLM / NTLMv2
`
40 : Comment appelle-t-on une charge malveillante Meterpreter dont les DLL sont toutes intégrées à celle-ci ?
StageLess
