TP4 : TCP, UDP et services réseau
Dans ce TP on va explorer un peu les protocoles TCP et UDP.

La première partie est détente, vous explorez TCP et UDP un peu, en vous servant de votre PC.

La seconde partie se déroule en environnement virtuel, avec des VMs. Les VMs vont nous permettre en place des services réseau, qui reposent sur TCP et UDP.
Le but est donc de commencer à mettre les mains de plus en plus du côté administration, et pas simple client.

Dans cette seconde partie, vous étudierez donc :

le protocole SSH (contrôle de machine à distance)
le protocole DNS (résolution de noms)
essentiel au fonctionnement des réseaux modernes
TCP UDP

Sommaire
TP4 : TCP, UDP et services réseau
Sommaire
0. Prérequis
I. First steps
II. Mise en place
1. SSH
2. Routage
III. DNS
1. Présentation
2. Setup
3. Test
0. Prérequis
➜ Pour ce TP, on va se servir de VMs Rocky Linux. On va en faire plusieurs, n'hésitez pas à diminuer la RAM (512Mo ou 1Go devraient suffire). Vous pouvez redescendre la mémoire vidéo aussi.

➜ Si vous voyez un 🦈 c'est qu'il y a un PCAP à produire et à mettre dans votre dépôt git de rendu

➜ L'emoji 🖥️ indique une VM à créer. Pour chaque VM, vous déroulerez la checklist suivante :

 Créer la machine (avec une carte host-only)
 Définir une IP statique à la VM
 Donner un hostname à la machine
 Vérifier que l'accès SSH fonctionnel
 Vérifier que le firewall est actif
 Remplir votre fichier hosts, celui de votre PC, pour accéder au VM avec un nom
 Dès que le routeur est en place, n'oubliez pas d'ajouter une route par défaut aux autres VM pour qu'elles aient internet
Toutes les commandes pour réaliser ces opérations sont dans le mémo Rocky. Aucune de ces étapes ne doit figurer dan le rendu, c'est juste la mise en place de votre environnement de travail.

I. First steps
Faites-vous un petit top 5 des applications que vous utilisez sur votre PC souvent, des applications qui utilisent le réseau : un site que vous visitez souvent, un jeu en ligne, Spotify, j'sais po moi, n'importe.

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

avec Wireshark, on va faire les chirurgiens réseau

déterminez, pour chaque application :

IP et port du serveur auquel vous vous connectez
le port local que vous ouvrez pour vous connecter
Spotify  ip: 35.198.224.25     port: 62993 TCP
soundcloud   ip: 52.222.158.97 port: 443   TCP
outlook      ip: 52.109.32.24  port: 443   TCP
instagram    ip: 157.240.21.63 port: 443   TCP
discord      ips: 162.159.138.232 port: 443 TCP
                  162.159.128.233
                  162.159.135.232
                  162.159.137.232
                  (ouai discord à plusieurs ip vu dans la réponse dns de discord.com.)
Dès qu'on se connecte à un serveur, notre PC ouvre un port random. Une fois la connexion TCP ou UDP établie, entre le port de notre PC et le port du serveur qui est en écoute, on parle de tunnel TCP ou de tunnel UDP.

Aussi, TCP ou UDP ? Comment le client sait ? Il sait parce que le serveur a décidé ce qui était le mieux pour tel ou tel type de trafic (un jeu, une page web, etc.) et que le logiciel client est codé pour utiliser TCP ou UDP en conséquence.

🌞 Demandez l'avis à votre OS

votre OS est responsable de l'ouverture des ports, et de placer un programme en "écoute" sur un port
il est aussi responsable de l'ouverture d'un port quand une application demande à se connecter à distance vers un serveur
bref il voit tout quoi
utilisez la commande adaptée à votre OS pour repérer, dans la liste de toutes les connexions réseau établies, la connexion que vous voyez dans Wireshark, pour chacune des 5 applications
Il faudra ajouter des options adaptées aux commandes pour y voir clair. Pour rappel, vous cherchez des connexions TCP ou UDP.

# MacOS
$ netstat

# GNU/Linux
$ ss

# Windows
$ netstat
🦈🦈🦈🦈🦈 Bah ouais, captures Wireshark à l'appui évidemment. Une capture pour chaque application, qui met bien en évidence le trafic en question.

II. Mise en place
1. SSH
🖥️ Machine node1.tp4.b1

n'oubliez pas de dérouler la checklist (voir les prérequis du TP)
donnez lui l'adresse IP 10.4.1.11/24
Connectez-vous en SSH à votre VM.

🌞 Examinez le trafic dans Wireshark

déterminez si SSH utilise TCP ou UDP

pareil réfléchissez-y deux minutes, logique qu'on utilise pas UDP non ?
4	0.000584	10.4.1.11	10.4.1.1	TCP	66	22 → 51395 [SYN, ACK] Seq=0 Ack=1 Win=64240 Len=0 MSS=1460 SACK_PERM=1 WS=128
il s'agit du paquet d'initialisation de la connexion, elle est en TCP.
repérez le 3-Way Handshake à l'établissement de la connexion

c'est le SYN SYNACK ACK
repérez du trafic SSH

18	6.429800	10.4.1.1	10.4.1.11	SSHv2	134	Client: Encrypted packet (len=80)
19	6.438220	10.4.1.11	10.4.1.1	SSHv2	150	Server: Encrypted packet (len=96)
Un trafic SSH allant du client vers le server 
Un trafic SSH allant du server vers le client
repérez le FIN ACK à la fin d'une connexion
entre le 3-way handshake et l'échange FIN, c'est juste une bouillie de caca chiffré, dans un tunnel TCP
- 91	24.977136	10.4.1.1	10.4.1.11	TCP	54	51395 → 22 [FIN, ACK] Seq=3093 Ack=4046 Win=130304 Len=0
- 92	24.981164	10.4.1.11	10.4.1.1	TCP	60	22 → 51395 [FIN, ACK] Seq=4046 Ack=3094 Win=64128 Len=0

🌞 Demandez aux OS

repérez, avec une commande adaptée (netstat ou ss), la connexion SSH depuis votre machine
ET repérez la connexion SSH depuis votre VM
TCP    10.4.1.1:51456         10.4.1.11:22           ESTABLISHED
🦈 Je veux une capture clean avec le 3-way handshake, un peu de trafic au milieu et une fin de connexion

2. Routage
Ouais, un peu de répétition, ça fait jamais de mal. On va créer une machine qui sera notre routeur, et permettra à toutes les autres machines du réseau d'avoir Internet.

🖥️ Machine router.tp4.b1

n'oubliez pas de dérouler la checklist (voir les prérequis du TP)
donnez lui l'adresse IP 10.4.1.254/24 sur sa carte host-only
ajoutez-lui une carte NAT, qui permettra de donner Internet aux autres machines du réseau
référez-vous au TP précédent
Rien à remettre dans le compte-rendu pour cette partie.

III. DNS
1. Présentation
Un serveur DNS est un serveur qui est capable de répondre à des requêtes DNS.

Une requête DNS est la requête effectuée par une machine lorsqu'elle souhaite connaître l'adresse IP d'une machine, lorsqu'elle connaît son nom.

Par exemple, si vous ouvrez un navigateur web et saisissez https://www.google.com alors une requête DNS est automatiquement effectuée par votre PC pour déterminez à quelle adresse IP correspond le nom www.google.com.

La partie https:// ne fait pas partie du nom de domaine, ça indique simplement au navigateur la méthode de connexion. Ici, c'est HTTPS.

Dans cette partie, on va monter une VM qui porte un serveur DNS. Ce dernier répondra aux autres VMs du LAN quand elles auront besoin de connaître des noms. Ainsi, ce serveur pourra :

résoudre des noms locaux
vous pourrez ping node1.tp4.b1 et ça fonctionnera
mais aussi ping www.google.com et votre serveur DNS sera capable de le résoudre aussi
Dans la vraie vie, il n'est pas rare qu'une entreprise gère elle-même ses noms de domaine, voire gère elle-même son serveur DNS. C'est donc du savoir ré-utilisable pour tous qu'on voit ici.

En réalité, ce n'est pas votre serveur DNS qui pourra résoudre www.google.com, mais il sera capable de forward (faire passer) votre requête à un autre serveur DNS qui lui, connaît la réponse.

Haiku DNS

2. Setup
🖥️ Machine dns-server.tp4.b1

n'oubliez pas de dérouler la checklist (voir les prérequis du TP)
donnez lui l'adresse IP 10.4.1.201/24
Installation du serveur DNS :

# assurez-vous que votre machine est à jour
$ sudo dnf update -y

# installation du serveur DNS, son p'tit nom c'est BIND9
$ sudo dnf install -y bind bind-utils
La configuration du serveur DNS va se faire dans 3 fichiers essentiellement :

un fichier de configuration principal
/etc/named.conf
on définit les trucs généraux, comme les adresses IP et le port où on veu écouter
on définit aussi un chemin vers les autres fichiers, les fichiers de zone
un fichier de zone
/var/named/tp4.b1.db
je vous préviens, la syntaxe fait mal
on peut y définir des correspondances IP ---> nom
un fichier de zone inverse
/var/named/tp4.b1.rev
on peut y définir des correspondances nom ---> IP
➜ Allooooons-y, fichier de conf principal

# éditez le fichier de config principal pour qu'il ressemble à :
$ sudo cat /etc/named.conf
options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
[...]
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;
[...]
# référence vers notre fichier de zone
zone "tp4.b1" IN {
     type master;
     file "tp4.b1.db";
     allow-update { none; };
     allow-query {any; };
};
# référence vers notre fichier de zone inverse
zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp4.b1.rev";
     allow-update { none; };
     allow-query { any; };
};
➜ Et pour les fichiers de zone

# Fichier de zone pour nom -> IP

$ sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns-server IN A 10.4.1.201
node1      IN A 10.4.1.11
# Fichier de zone inverse pour IP -> nom

$ sudo cat /var/named/tp4.b1.rev

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

;Reverse lookup for Name Server
201 IN PTR dns-server.tp4.b1.
11 IN PTR node1.tp4.b1.
➜ Une fois ces 3 fichiers en place, démarrez le service DNS

# Démarrez le service tout de suite
$ sudo systemctl start named

# Faire en sorte que le service démarre tout seul quand la VM s'allume
$ sudo systemctl enable named

# Obtenir des infos sur le service
$ sudo systemctl status named

# Obtenir des logs en cas de probème
$ sudo journalctl -xe -u named
🌞 Dans le rendu, je veux

un cat des fichiers de conf
un systemctl status named qui prouve que le service tourne bien
une commande ss qui prouve que le service écoute bien sur un port
🌞 Ouvrez le bon port dans le firewall

grâce à la commande ss vous devrez avoir repéré sur quel port tourne le service
vous l'avez écrit dans la conf aussi toute façon :)
ouvrez ce port dans le firewall de la machine dns-server.tp4.b1 (voir le mémo réseau Rocky)
3. Test
🌞 Sur la machine node1.tp4.b1

configurez la machine pour qu'elle utilise votre serveur DNS quand elle a besoin de résoudre des noms
assurez vous que vous pouvez :
résoudre des noms comme node1.tp4.b1 et dns-server.tp4.b1
mais aussi des noms comme www.google.com
🌞 Sur votre PC

utilisez une commande pour résoudre le nom node1.tp4.b1 en utilisant 10.4.1.201 comme serveur DNS
Le fait que votre serveur DNS puisse résoudre un nom comme www.google.com, ça s'appelle la récursivité et c'est activé avec la ligne recursion yes; dans le fichier de conf.

🦈 Capture d'une requête DNS vers le nom node1.tp4.b1 ainsi que la réponse