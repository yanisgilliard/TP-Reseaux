TP3 : On va router des trucs
0. Prérequis
➜ Pour ce TP, on va se servir de VMs Rocky Linux. 1Go RAM c'est large large. Vous pouvez redescendre la mémoire vidéo aussi.
➜ Vous aurez besoin de deux réseaux host-only dans VirtualBox :

un premier réseau 10.3.1.0/24

le second 10.3.2.0/24

vous devrez désactiver le DHCP de votre hyperviseur (VirtualBox) et définir les IPs de vos VMs de façon statique

➜ Les firewalls de vos VMs doivent toujours être actifs (et donc correctement configurés).
➜ Si vous voyez le p'tit pote 🦈 c'est qu'il y a un PCAP à produire et à mettre dans votre dépôt git de rendu.

I. ARP
Première partie simple, on va avoir besoin de 2 VMs.



Machine
10.3.1.0/24




john
10.3.1.11


marcel
10.3.1.12




   john               marcel
  ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     │
  └─────┘    └───┘    └─────┘



Référez-vous au mémo Réseau Rocky pour connaître les commandes nécessaire à la réalisation de cette partie.


1. Echange ARP
🌞Générer des requêtes ARP

effectuer un ping d'une machine à l'autre
observer les tables ARP des deux machines
repérer l'adresse MAC de john dans la table ARP de marcel et vice-versa
prouvez que l'info est correcte (que l'adresse MAC que vous voyez dans la table est bien celle de la machine correspondante)

une commande pour voir la MAC de marcel dans la table ARP de john

et une commande pour afficher la MAC de marcel, depuis marcel





2. Analyse de trames
🌞Analyse de trames

utilisez la commande tcpdump pour réaliser une capture de trame
videz vos tables ARP, sur les deux machines, puis effectuez un ping


🦈 Capture réseau tp3_arp.pcapng qui contient un ARP request et un ARP reply

Si vous ne savez pas comment récupérer votre fichier .pcapng sur votre hôte afin de l'ouvrir dans Wireshark, et me le livrer en rendu, demandez-moi.


II. Routage
Vous aurez besoin de 3 VMs pour cette partie. Réutilisez les deux VMs précédentes.



Machine
10.3.1.0/24
10.3.2.0/24




router
10.3.1.254
10.3.2.254


john
10.3.1.11
no


marcel
no
10.3.2.12




Je les appelés marcel et john PASKON EN A MAR des noms nuls en réseau 🌻


   john                router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └───┘    └─────┘    └───┘    └─────┘



1. Mise en place du routage
🌞Activer le routage sur le noeud router

Cette étape est nécessaire car Rocky Linux c'est pas un OS dédié au routage par défaut. Ce n'est bien évidemment une opération qui n'est pas nécessaire sur un équipement routeur dédié comme du matériel Cisco.

🌞Ajouter les routes statiques nécessaires pour que john et marcel puissent se ping

il faut taper une commande ip route add pour cela, voir mémo
il faut ajouter une seule route des deux côtés
une fois les routes en place, vérifiez avec un ping que les deux machines peuvent se joindre



2. Analyse de trames
🌞Analyse des échanges ARP

videz les tables ARP des trois noeuds
effectuez un ping de john vers marcel

regardez les tables ARP des trois noeuds
essayez de déduire un peu les échanges ARP qui ont eu lieu
répétez l'opération précédente (vider les tables, puis ping), en lançant tcpdump sur marcel


écrivez, dans l'ordre, les échanges ARP qui ont eu lieu, puis le ping et le pong, je veux TOUTES les trames utiles pour l'échange

Par exemple (copiez-collez ce tableau ce sera le plus simple) :



ordre
type trame
IP source
MAC source
IP destination
MAC destination




1
Requête ARP
x

marcel AA:BB:CC:DD:EE

x
Broadcast FF:FF:FF:FF:FF



2
Réponse ARP
x
?
x

marcel AA:BB:CC:DD:EE



...
...
...
...




?
Ping
?
?
?
?


?
Pong
?
?
?
?




Vous pourriez, par curiosité, lancer la capture sur john aussi, pour voir l'échange qu'il a effectué de son côté.

🦈 Capture réseau tp3_routage_marcel.pcapng

3. Accès internet
🌞Donnez un accès internet à vos machines

ajoutez une carte NAT en 3ème inteface sur le router pour qu'il ait un accès internet
ajoutez une route par défaut à john et marcel

vérifiez que vous avez accès internet avec un ping

le ping doit être vers une IP, PAS un nom de domaine


donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser

vérifiez que vous avez une résolution de noms qui fonctionne avec dig

puis avec un ping vers un nom de domaine



🌞Analyse de trames

effectuez un ping 8.8.8.8 depuis john

capturez le ping depuis john avec tcpdump

analysez un ping aller et le retour qui correspond et mettez dans un tableau :




ordre
type trame
IP source
MAC source
IP destination
MAC destination





1
ping

marcel 10.3.1.12


marcel AA:BB:CC:DD:EE

8.8.8.8
?



2
pong
...
...
...
...
...



🦈 Capture réseau tp3_routage_internet.pcapng

III. DHCP
On reprend la config précédente, et on ajoutera à la fin de cette partie une 4ème machine pour effectuer des tests.



Machine
10.3.1.0/24
10.3.2.0/24




router
10.3.1.254
10.3.2.254


john
10.3.1.11
no


bob
oui mais pas d'IP statique
no


marcel
no
10.3.2.12




   john               router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └─┬─┘    └─────┘    └───┘    └─────┘
   john        │
  ┌─────┐      │
  │     │      │
  │     ├──────┘
  └─────┘



1. Mise en place du serveur DHCP
🌞Sur la machine john, vous installerez et configurerez un serveur DHCP (go Google "rocky linux dhcp server").

installation du serveur sur john

créer une machine bob

faites lui récupérer une IP en DHCP à l'aide de votre serveur


Il est possible d'utilise la commande dhclient pour forcer à la main, depuis la ligne de commande, la demande d'une IP en DHCP, ou renouveler complètement l'échange DHCP (voir dhclient -h puis call me et/ou Google si besoin d'aide).

🌞Améliorer la configuration du DHCP

ajoutez de la configuration à votre DHCP pour qu'il donne aux clients, en plus de leur IP :

une route par défaut
un serveur DNS à utiliser


récupérez de nouveau une IP en DHCP sur bob pour tester :


marcel doit avoir une IP

vérifier avec une commande qu'il a récupéré son IP
vérifier qu'il peut ping sa passerelle


il doit avoir une route par défaut

vérifier la présence de la route avec une commande
vérifier que la route fonctionne avec un ping vers une IP


il doit connaître l'adresse d'un serveur DNS pour avoir de la résolution de noms

vérifier avec la commande dig que ça fonctionne
vérifier un ping vers un nom de domaine






2. Analyse de trames
🌞Analyse de trames

lancer une capture à l'aide de tcpdump afin de capturer un échange DHCP
demander une nouvelle IP afin de générer un échange DHCP
exportez le fichier .pcapng


🦈 Capture réseau tp3_dhcp.pcapng