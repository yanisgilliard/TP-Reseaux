# TP1 - Premier pas réseau
## I. Exploration locale en solo
### 1. Affichage d'informations sur la pile TCP/IP locale 
#### Interface Wifi : 
commande : ipconfig /all
```` 
   Description. . . . . . . . . . . . . . : Realtek 8821CE Wireless LAN 802.11ac PCI-E NIC
   Adresse physique . . . . . . . . . . . : 90-E8-68-62-F0-A7
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.17.12 
````
#### Interface Ethernet : 
Je ne possède pas de carte Ethernet 

#### Affichez votre gateway
commande : ipconfig /all
````
 Passerelle par défaut. . . . . . . . . : 10.33.19.254
````
#### Déterminer la MAC de la passerelle
commande : arp -a 
````
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
````
#### Trouvez comment afficher les informations sur une carte IP 
````
Suffixe DNS propre à la connexion: 
Adresse physique: 90-E8-68-62-F0-A7
Adresse IPv4: 10.33.17.12
Passerelle par défaut IPv4: 10.33.19.254
````

### 2. Modifications des informations

#### A. Modification d'adresse IP (part 1)

##### Utilisez l'interface graphique de votre OS pour changer d'adresse IP : 

![](https://i.imgur.com/2gJzWFb.png)

##### Il est possible que vous perdiez l'accès internet : 
J'ai perdu l'accès à Internet en changeant d'Ip, le nouvel Ip que j'ai sélectionné appartenait déjà à quelqu'un alors il a la priorité car il était connecté avant moi je n'ai donc pas pu me connecter. 
](https://)

## II. Exploration locale en duo
Owkay. Vous savez à ce stade :

afficher les informations IP de votre machine
modifier les informations IP de votre machine
c'est un premier pas vers la maîtrise de votre outil de travail
On va maintenant répéter un peu ces opérations, mais en créant un réseau local de toutes pièces : entre deux PCs connectés avec un câble RJ45.

1. Prérequis
deux PCs avec ports RJ45
un câble RJ45
firewalls désactivés sur les deux PCs
2. Câblage
Ok c'est la partie tendue. Prenez un câble. Branchez-le des deux côtés. Bap.

Création du réseau (oupa)
Cette étape pourrait paraître cruciale. En réalité, elle n'existe pas à proprement parlé. On ne peut pas "créer" un réseau.

Si une machine possède une carte réseau, et si cette carte réseau porte une adresse IP, alors cette adresse IP se trouve dans un réseau (l'adresse de réseau). Ainsi, le réseau existe. De fait.

Donc il suffit juste de définir une adresse IP sur une carte réseau pour que le réseau existe ! Bap.

### 3. Modification d'adresse IP
🌞 Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau

Si vos PCs ont un port RJ45 alors y'a une carte réseau Ethernet associée
choisissez une IP qui commence par "10.10.10."
/24 pour la longueur de masque, ou 255.255.255.0 pour le masque (suivant les OS, l'info est demandée différement, mais c'est la même chose)
🌞 Vérifier à l'aide d'une commande que votre IP a bien été changée

````
Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::a0f0:514f:c687:c412%7
   Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
````

🌞 Vérifier que les deux machines se joignent

````
>ping 10.10.10.2

Envoi d’une requête 'Ping'  10.10.10.2 avec 32 octets de données :
Réponse de 10.10.10.2 : octets=32 temps=4 ms TTL=64
Réponse de 10.10.10.2 : octets=32 temps=2 ms TTL=64
Réponse de 10.10.10.2 : octets=32 temps=2 ms TTL=64
Réponse de 10.10.10.2 : octets=32 temps=2 ms TTL=64
````

utilisez la commande ping pour tester la connectivité entre les deux machines
La commande ping est un message simple envoyé à une autre machine. Cette autre machine retournera alors un message tout aussi simple. ping utilise un protocole frère de IP : le protocole ICMP. On mesure souvent la latence réseau grâce à un ping : en mesurant la durée entre l'émission du ping et la réception du retour.

🌞 Déterminer l'adresse MAC de votre correspondant

pour cela, affichez votre table ARP
````
>arp -a

Interface : 10.10.10.1 --- 0x7
  Adresse Internet      Adresse physique      Type
  10.10.10.2            c0-18-50-0e-cb-bb     dynamique
  10.10.10.255          ff-ff-ff-ff-ff-ff     statique
````

### 4. Utilisation d'un des deux comme gateway
Ca, ça peut toujours dépann irl. Comme pour donner internet à une tour sans WiFi quand y'a un PC portable à côté, par exemple.

L'idée est la suivante :

vos PCs ont deux cartes avec des adresses IP actuellement
la carte WiFi, elle permet notamment d'aller sur internet, grâce au réseau YNOV
la carte Ethernet, qui permet actuellement de joindre votre coéquipier, grâce au réseau que vous avez créé :)
si on fait un tit schéma tout moche, ça donne ça :
````
Internet           Internet
     |                   |
    WiFi                WiFi
     |                   |
    PC 1 ---Ethernet--- PC 2
````
    
- internet joignable en direct par le PC 1
- internet joignable en direct par le PC 2
vous allez désactiver Internet sur une des deux machines, et vous servir de l'autre machine pour accéder à internet.
```` 
 Internet           Internet
     X                   |
     X                  WiFi
     |                   |
    PC 1 ---Ethernet--- PC 2
````
    
- internet joignable en direct par le PC 2
- internet joignable par le PC 1, en passant par le PC 2
pour ce faiiiiiire :
désactivez l'interface WiFi sur l'un des deux postes
s'assurer de la bonne connectivité entre les deux PCs à travers le câble RJ45
sur le PC qui n'a plus internet
sur la carte Ethernet, définir comme passerelle l'adresse IP de l'autre PC
sur le PC qui a toujours internet
sur Windows, il y a une option faite exprès (google it. "share internet connection windows 10" par exemple)
sur GNU/Linux, faites le en ligne de commande ou utilisez Network Manager (souvent présent sur tous les GNU/Linux communs)
sur MacOS : toute façon vous avez pas de ports RJ, si ? :o (google it sinon)
🌞Tester l'accès internet

pour tester la connectivité à internet on fait souvent des requêtes simples vers un serveur internet connu
essayez de ping l'adresse IP 1.1.1.1, c'est un serveur connu de CloudFlare (demandez-moi si vous comprenez pas trop la démarche)
![](https://i.imgur.com/m0dWc7l.png)


````
ip a                                                    
2: enp3s0: 
    link/ether c0:18:50:0e:cb:bb
    inet 192.168.137.2/24
````
````
ping 1.1.1.1 
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=54 time=21.4 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=54 time=21.7 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=54 time=22.6 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=54 time=21.1 ms
^C
--- 1.1.1.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 21.094/21.691/22.601/0.565 ms
````
https://github.com/Ayshyy/TP-R-seau-1/blob/main/imgs/Partage_de_co.png

Source: https://prograide.com/pregunta/48209/comment-afficher-une-image-locale-dans-markdown

🌞 Prouver que la connexion Internet passe bien par l'autre PC

utiliser la commande traceroute ou tracert (suivant votre OS) pour bien voir que les requêtes passent par la passerelle choisie (l'autre le PC)
La commande traceroute retourne la liste des machines par lesquelles passent le ping avant d'atteindre sa destination.

````
traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  _gateway (192.168.137.1)  1.000 ms  0.959 ms  0.941 ms
 2  * * *
 3  _gateway (10.33.19.254)  8.078 ms  8.063 ms  8.047 ms
 4  137.149.196.77.rev.sfr.net (77.196.149.137)  9.263 ms  9.247 ms  9.231 ms
 5  108.97.30.212.rev.sfr.net (212.30.97.108)  9.215 ms  9.199 ms  9.184 ms
 6  222.172.136.77.rev.sfr.net (77.136.172.222)  23.432 ms  24.416 ms  24.383 ms
 7  221.172.136.77.rev.sfr.net (77.136.172.221)  24.366 ms  23.280 ms  23.240 ms
 8  221.10.136.77.rev.sfr.net (77.136.10.221)  23.223 ms  22.025 ms  26.490 ms
 9  221.10.136.77.rev.sfr.net (77.136.10.221)  21.969 ms  21.954 ms  21.939 ms
10  141.101.67.254 (141.101.67.254)  21.923 ms  20.340 ms  20.718 ms
11  172.71.124.2 (172.71.124.2)  24.857 ms 172.71.116.2 (172.71.116.2)  24.840 ms 172.71.124.2 (172.71.124.2)  23.739 ms
12  one.one.one.one (1.1.1.1)  23.683 ms  23.665 ms  23.646 ms
5. Petit chat privé
Netcat
````

On va créer un chat extrêmement simpliste à l'aide de netcat (abrégé nc). Il est souvent considéré comme un bon couteau-suisse quand il s'agit de faire des choses avec le réseau.

Sous GNU/Linux et MacOS vous l'avez sûrement déjà, sinon débrouillez-vous pour l'installer :). Les Windowsien, ça se passe ici (from https://eternallybored.org/misc/netcat/).

Une fois en possession de netcat, vous allez pouvoir l'utiliser en ligne de commande. Comme beaucoup de commandes sous GNU/Linux, Mac et Windows, on peut utiliser l'option -h (h pour help) pour avoir une aide sur comment utiliser la commande.

Sur un Windows, ça donne un truc comme ça :

````
C:\Users\It4\Desktop\netcat-win32-1.11>nc.exe -h
[v1.11 NT www.vulnwatch.org/netcat/]
connect to somewhere:   nc [-options] hostname port[s] [ports] ...
listen for inbound:     nc -l -p port [options] [hostname] [port]
options:
        -d              detach from console, background mode

        -e prog         inbound program to exec [dangerous!!]
        -g gateway      source-routing hop point[s], up to 8
        -G num          source-routing pointer: 4, 8, 12, ...
        -h              this cruft
        -i secs         delay interval for lines sent, ports scanned
        -l              listen mode, for inbound connects
        -L              listen harder, re-listen on socket close
        -n              numeric-only IP addresses, no DNS
        -o file         hex dump of traffic
        -p port         local port number
        -r              randomize local and remote ports
        -s addr         local source address
        -t              answer TELNET negotiation
        -u              UDP mode
        -v              verbose [use twice to be more verbose]
        -w secs         timeout for connects and final net reads
        -z              zero-I/O mode [used for scanning]
port numbers can be individual or ranges: m-n [inclusive]
````


L'idée ici est la suivante :

l'un de vous jouera le rôle d'un serveur
l'autre sera le client qui se connecte au serveur
Précisément, on va dire à netcat d'écouter sur un port. Des ports, y'en a un nombre fixe (65536, on verra ça plus tard), et c'est juste le numéro de la porte à laquelle taper si on veut communiquer avec le serveur.

Si le serveur écoute à la porte 20000, alors le client doit demander une connexion en tapant à la porte numéro 20000, simple non ?

Here we go :

🌞 sur le PC serveur avec par exemple l'IP 192.168.1.1

nc.exe -l -p 8888
"netcat, écoute sur le port numéro 8888 stp"
il se passe rien ? Normal, faut attendre qu'un client se connecte
🌞 sur le PC client avec par exemple l'IP 192.168.1.2

nc.exe 192.168.1.1 8888
"netcat, connecte toi au port 8888 de la machine 192.168.1.1 stp"
une fois fait, vous pouvez taper des messages dans les deux sens
appelez-moi quand ça marche ! :)
si ça marche pas, essayez d'autres options de netcat
````
PS C:\Users\valen\Desktop\netcat-1.11> .\nc64.exe 192.168.137.2 8888
Coucouu
Hello
````
🌞 Visualiser la connexion en cours

sur tous les OS, il existe une commande permettant de voir les connexions en cours
ouvrez un deuxième terminal pendant une session netcat, et utilisez la commande correspondant à votre OS pour repérer la connexion netcat :
# Windows (dans un Powershell administrateur)
````
$ netstat -a -n -b
  TCP    192.168.137.1:57007    192.168.137.2:8888     ESTABLISHED
 [nc64.exe]
````
🌞 Pour aller un peu plus loin

si vous faites un netstat sur le serveur AVANT que le client netcat se connecte, vous devriez observer que votre serveur netcat écoute sur toutes vos interfaces
c'est à dire qu'on peut s'y connecter depuis la wifi par exemple :D
il est possible d'indiquer à netcat une interface précise sur laquelle écouter
par exemple, on écoute sur l'interface Ethernet, mais pas sur la WiFI
#### Sur Windows/MacOS
````
$ nc.exe -l -p PORT_NUMBER -s IP_ADDRESS
Par exemple
$ nc.exe -l -p 9999 -s 192.168.1.37
````

### 6. Firewall
Toujours par 2.

Le but est de configurer votre firewall plutôt que de le désactiver

🌞 Activez et configurez votre firewall

autoriser les ping
configurer le firewall de votre OS pour accepter le ping
aidez vous d'internet
on rentrera dans l'explication dans un prochain cours mais sachez que ping envoie un message ICMP de type 8 (demande d'ECHO) et reçoit un message ICMP de type 0 (réponse d'écho) en retour
autoriser le traffic sur le port qu'utilise nc
on parle bien d'ouverture de port TCP et/ou UDP
on ne parle PAS d'autoriser le programme nc
choisissez arbitrairement un port entre 1024 et 20000
vous utiliserez ce port pour communiquer avec netcat par groupe de 2 toujours
le firewall du PC serveur devra avoir un firewall activé et un netcat qui fonctionne
** Le pare feu Windows**

````
Panneau de configuration > Tous les Panneaux de configuration > Pare-feu Windows Defender >> Paramètre avancé
Règles Pare feu
````
![](https://i.imgur.com/v0dUxHt.png)

Créé une règle qui accepte les ping imcp
Règles IMCP
![](https://i.imgur.com/9rF6n9S.png)



## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP

#### Exploration du DHCP, depuis votre PC
 
Adresse IP du serveur DHCP du réseau WiFi YNOV
````
Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
````
Date d'expiration de votre bail DHCP
````
Suffixe DNS propre à la connexion: 
Bail obtenu: mardi 4 octobre 2022 14:00:08
Bail expirant: mercredi 5 octobre 2022 14:00:07
````

### 2. DNS 
````
PS C:\Users\yanis> nslookup google.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:818::200e
          142.250.179.78
````
```` 
PS C:\Users\yanis> nslookup ynov.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          2606:4700:20::681a:ae9
          104.26.10.233
          172.67.74.226
          104.26.11.233
```` 

Google est sur son propre serveur alors que ynov lui est sur le serveur de google. Google a moins d'adresses affichés que Ynov mais ce n'est pas pour autant qu'il en possède moins, c'est juste qu'ils sont affichés a d'autres niveaux.  


Reverse lookup :
J'ai changé l'IP car celle du TP n'existait pas.
````
PS C:\Users\yanis> nslookup 219.34.113.12
Serveur :   dns.google
Address:  8.8.8.8

Nom :    softbank219034113012.bbtec.net
Address:  219.34.113.12
````

````
PS C:\Users\yanis> nslookup 78.34.2.17
Serveur :   dns.google
Address:  8.8.8.8

Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
````

Chaque adresse web est unique (IP) et est accéssible via le serveur de Google. 

## IV. Wireshark
Wireshark est un outil qui permet de visualiser toutes les trames qui sortent et entrent d'une carte réseau.

On appelle ça un sniffer, ou analyseur de trames.

Wireshark

Il peut :

enregistrer le trafic réseau, pour l'analyser plus tard
afficher le trafic réseau en temps réel
On peut TOUT voir.

Un peu austère aux premiers abords, une manipulation très basique permet d'avoir une très bonne compréhension de ce qu'il se passe réellement.

➜ Téléchargez l'outil Wireshark.

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

un ping entre vous et votre passerelle
un netcat entre vous et votre mate, branché en RJ45
une requête DNS. Identifiez dans la capture le serveur DNS à qui vous posez la question.
prenez moi des screens des trames en question
on va prendre l'habitude d'utiliser Wireshark souvent dans les cours, pour visualiser ce qu'il se passe
IMCP
![](https://i.imgur.com/qjYosHv.png)


NETCAT
![](https://i.imgur.com/KMXhQDk.png)


DNS (nslookup)
![](https://i.imgur.com/QNA86jJ.png)