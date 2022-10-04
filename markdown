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
Adresse physique: ‎90-E8-68-62-F0-A7
Adresse IPv4: 10.33.17.12
Passerelle par défaut IPv4: 10.33.19.254
````

### 2. Modifications des informations

#### A. Modification d'adresse IP (part 1)

##### Utilisez l'interface graphique de votre OS pour changer d'adresse IP : 

![](https://i.imgur.com/2gJzWFb.png)

##### Il est possible que vous perdiez l'accès internet : 
J'ai perdu l'accès à Internet en changeant d'Ip, le nouvel Ip que j'ai sélectionné appartenait déjà à quelqu'un alors il a la priorité car il était connecté avant moi je n'ai donc pas pu me connecter. 
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
Chaque site possède son adresse unique (IP) et est accéssible via le serveur de Google.