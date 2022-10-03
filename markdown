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
