# TP2 : Ethernet, IP, et ARP
## I. Setup IP 
 
````
Adresse Reseau           = 172.16.0.0
Adresse Broadcast        = 172.16.3.255
Masque de Sous-Reseau    = 255.255.252.0
Nombre de Machines       = 1022
Premiere machine         = 172.16.0.1
Derniere machine         = 172.16.3.254
--------------------- OU -----------------
Premiere machine         = 172.16.0.0
Derniere machine         = 172.16.3.255
```` 
### Mettez en place une configuration réseau fonctionnelle entre les deux machines :

````
netsh interface ipv4 set address name="Ethernet" static 172.16.0.0 255.255.252.0 172.16.3.254
````

### Prouvez que la connexion est fonctionnelle entre les deux machines : 
````
PS C:\WINDOWS\system32> netsh interface ipv4 set address name="Ethernet 2" static 172.16.0.3 255.255.252.0 172.16.31.254

PS C:\WINDOWS\system32> ping 172.16.0.2

Envoi d’une requête 'Ping'  172.16.0.2 avec 32 octets de données :
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 172.16.0.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 1ms, Moyenne = 1ms
````

### Wireshark it
Ping Wireshark 


## II. ARP my bro
### Check the ARP table

````
PS C:\WINDOWS\system32> arp -a

Interface : 192.168.75.1 --- 0x7
  Adresse Internet      Adresse physique      Type
  192.168.75.254        00-50-56-e0-75-3c     dynamique
  192.168.75.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 10.33.16.172 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.33.18.221          78-4f-43-87-f5-11     dynamique
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  10.33.19.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.200.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.200.254       00-50-56-e2-26-52     dynamique
  192.168.200.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 172.16.0.3 --- 0x30
  Adresse Internet      Adresse physique      Type
  172.16.0.2            b4-45-06-7e-a1-3d     dynamique
  172.16.3.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
````
````
Interface : 172.16.0.3 --- 0x30
  Adresse Internet      Adresse physique      Type
  172.16.0.2            b4-45-06-7e-a1-3d     dynamique
````
````
Interface : 10.33.16.172 --- 0xc
  Adresse Internet      Adresse physique      
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
````

### Manipuler la table ARP 
````
PS C:\WINDOWS\system32> arp -d
````
````
PS C:\WINDOWS\system32> arp -a

Interface : 192.168.75.1 --- 0x7
  Adresse Internet      Adresse physique      Type
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.16.172 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.200.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 172.16.0.3 --- 0x30
  Adresse Internet      Adresse physique      Type
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
````
````
PS C:\WINDOWS\system32> ping 172.16.0.2

Envoi d’une requête 'Ping'  172.16.0.2 avec 32 octets de données :
Réponse de 172.16.0.2 : octets=32 temps=2 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128
Réponse de 172.16.0.2 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 172.16.0.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 2ms, Moyenne = 1ms
PS C:\WINDOWS\system32> arp -a

Interface : 192.168.75.1 --- 0x7
  Adresse Internet      Adresse physique      Type
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.16.172 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  10.33.19.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.200.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 172.16.0.3 --- 0x30
  Adresse Internet      Adresse physique      Type
  172.16.0.2            b4-45-06-7e-a1-3d     dynamique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
````

## Wireshark it
ARP Wireshark 

## III. DHCP you too my brooo
DHCP Wireshark
