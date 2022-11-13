TP5 : VPN 

Etape 1 :
```` PS C:\Users\yanis> ssh rocky@54.226.120.206 ````

Etape 2 :
```` PS C:\Users\yanis> ssh-keygen -t rsa -b 4096 ```` 

Etape 3 :
```` [rocky@ip-172-31-27-226 ~]$ sudo dnf install elrepo-release epel-release ````
```` [rocky@ip-172-31-27-226 ~]$ sudo dnf install kmod-wireguard wireguard-tools ````

Etape 4 : 
```` [rocky@ip-172-31-27-226 ~]$ wg genkey | sudo tee /etc/wireguard/private.key ````

Etape 5 :
```` [rocky@ip-172-31-27-226 ~]$ sudo chmod go= /etc/wireguard/private.key ````
```` [rocky@ip-172-31-27-226 ~]$ wg genkey | sudo tee /etc/wireguard/private.key ````

Etape 6 :
```` [rocky@ip-172-31-27-226 ~]$ wg genkey | sudo tee /etc/wireguard/private.key ````

Etape 7 :
```` [rocky@ip-172-31-27-226 ~]$ sudo chmod go= /etc/wireguard/private.key ````
```` [rocky@ip-172-31-27-226 ~]$ sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key ````

Etape 8 :
```` [rocky@ip-172-31-27-226 ~]$ sudo vi /etc/wireguard/wg0.conf ````
```` 
[Interface]
PrivateKey = kAsF6+2JjCLXfrwpTrEvTtBZjfNl5/6fCbN1pXPFDEQ=
Address = 10.8.0.1/24
ListenPort = 51820
SaveConfig = true 
````

Changement d'IP :
```` PS C:\Users\yanis> ssh rocky@54.226.120.206 ````

Etape 9 :
```` [rocky@ip-172-31-27-226 ~]$ sudo vi /etc/sysctl.conf ````
````
# sysctl settings are defined through files in
# /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.
#
# Vendors settings live in /usr/lib/sysctl.d/.
# To override a whole file, create a new file with the same in
# /etc/sysctl.d/ and put new settings there. To override
# only specific settings, add a file with a lexically later
# name in /etc/sysctl.d/ and put new settings there.
#
# For more information, see sysctl.conf(5) and sysctl.d(5).
net.ipv4.ip_forward=1
````

Etape 10 : 
```` 
[rocky@ip-172-31-27-226 ~]$ sudo firewall-cmd --zone=public --add-port=51820/udp --permanent
sudo: firewall-cmd: command not found
[rocky@ip-172-31-27-226 ~]$ sudo firewall-cmd --zone=internal --add-interface=wg0 --permanent
sudo: firewall-cmd: command not found 
````