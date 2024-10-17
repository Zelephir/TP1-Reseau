# I. Setup
➜ Mettre en place le setup suivant :

➜ Ptit tableau avec les adresses IP

Nom de la machine
IP dans le LAN 10.5.1.0/24

Votre PC
10.5.1.1

client1.tp5.b1
10.5.1.11

client2.tp5.b1
10.5.1.12

routeur.tp5.b1
10.5.1.254

/24 correspond à un masque de 255.255.255.0. C'est deux façons différentes d'écrire la même chose, on voit ça bientôt. Il faudra indiquer le même masque à toutes les machines.

➜ Il faut donc, sur votre PC :

créer un host-only (réseau privé-hôte en français)
définir l'IP 10.5.1.1/24 pour le PC depuis l'interface de VBox
ajouter les cartes réseau nécessaires à toutes les VMs

1 carte host-only pour les 3 VMs (clients et routeur)
1 carte NAT en + pour le routeur

Pour rappel la carte host-only, comme montrée sur le schéma, permet que tout le monde soit connecté à un switch virtuel, et ainsi former un LAN (réseau local). La carte NAT permet uniquement un accès internet.

➜ Ensuite, pour chaque VM...

configurer l'adresse IP demandée

ça se fait depuis la VM directement : chaque client choisit sa propre IP comme toujours !
mettez l'IP indiquée dans le tableau, et le même masque pour tout le monde

configurer un hostname pour la VM

comme ça, quand on est dans un terminal, le nom de la machin est affiché, et on sait où on est !
c'est affiché dans le prompt dans votre terminal : [it4@localhost]$

le nom par défaut c'est localhost et c'est pourri !


☀️ Uniquement avec des commandes, prouvez-que :

vous avez bien configuré les adresses IP demandées (un ip a suffit hein)

mael@client1:~/Desktop$ ip a
```powershell
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a3:73:6b brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.11/24 brd 10.5.1.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fea3:736b/64 scope link 
       valid_lft forever preferred_lft forever

```

mael@client2:~/Desktop$ ip a
```powershell
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:87:1d:4e brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.12/24 brd 10.5.1.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe87:1d4e/64 scope link 
       valid_lft forever preferred_lft forever

```

routeur :
```powershell
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1d:48:24 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.254/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1d:4824/64 scope link 
       valid_lft forever preferred_lft forever

```
vous avez bien configuré les hostnames demandés
```powershell
mael@client1:~/Desktop$ hostnamectl
 Static hostname: client1
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 25d95201e2c24e3bbe983cb99358dbe2
         Boot ID: ac177307372148dbbeabd6a9a5810e7e
  Virtualization: oracle
Operating System: Ubuntu 24.04.1 LTS              
          Kernel: Linux 6.8.0-45-generic
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Firmware Version: VirtualBox
   Firmware Date: Fri 2006-12-01
    Firmware Age: 17y 10month 2w
``` 
```powershell
mael@client2:~$ hostnamectl
 Static hostname: client2
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 25d95201e2c24e3bbe983cb99358dbe2
         Boot ID: de9a0b9c12c1441ea7293182df326794
  Virtualization: oracle
Operating System: Ubuntu 24.04.1 LTS              
          Kernel: Linux 6.8.0-45-generic
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Firmware Version: VirtualBox
   Firmware Date: Fri 2006-12-01
    Firmware Age: 17y 10month 2w   
```
```powershell
PING 10.5.1.12 (10.5.1.12) 56(84) bytes of data.
64 bytes from 10.5.1.12: icmp_seg=1 ttl-64 time-56.8 ms
54 bytes from 10.5.1.12: icmp_seg=2 ttl-64 time=2.22 ms
64 bytes from 10.5.1.12: icmp_seq=3 ttl-64 time-2.04 ms
64 bytes from 10.5.1.12 : icmp_seq=4 ttl=64 time=2.36 ms
64 bytes from 10.5.1.12: icmp_seq=5 ttl=64 time-2..85 ms
64 bytes from 10.5.1. 12: icmp_seq-6 tt1=64 time=1.61 ms
18.5.1. 12 ping statistics
rivéc packets transmitted, 6 received, % packet loss, t ime 5813hns
rtt min/avg/max/mdey = 1.611/11.181/56.800/20.482 s
Lroot@routeur "l# -
```
```powershell
^Cmael@client1:~/Desktop$ ping 10.5.1.12
PING 10.5.1.12 (10.5.1.12) 56(84) bytes of data.
64 bytes from 10.5.1.12: icmp_seq=1 ttl=64 time=8.29 ms
64 bytes from 10.5.1.12: icmp_seq=2 ttl=64 time=1.28 ms
64 bytes from 10.5.1.12: icmp_seq=3 ttl=64 time=1.38 ms
^C
--- 10.5.1.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2010ms
rtt min/avg/max/mdev = 1.284/3.650/8.292/3.282 ms

```

```powershell
mael@client2:~$ ping 10.5.1.11
PING 10.5.1.11 (10.5.1.11) 56(84) bytes of data.
64 bytes from 10.5.1.11: icmp_seq=1 ttl=64 time=1.80 ms
64 bytes from 10.5.1.11: icmp_seq=2 ttl=64 time=1.69 ms
64 bytes from 10.5.1.11: icmp_seq=3 ttl=64 time=0.894 ms
^C
--- 10.5.1.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2015ms
rtt min/avg/max/mdev = 0.894/1.460/1.798/0.402 ms

```
tout le monde peut se ping au sein du réseau 10.5.1.0/24

# II. Accès internet pour tous
➜ Actuellement, tout le monde est connecté, mais les clients n'ont pas internet !

➜ Dans cette partie, on va faire en sorte que tout le monde ait un accès internet :

le routeur a déjà un accès internet
les clients vont se servir du routeur comme passerelle afin d'accéder à internet

Pour ça :

il faut que notre machine routeur.tp5.b1 accepte de router des paquets
il faut que chaque client

connaisse routeur.tp.b1 comme sa passerelle (10.5.1.254)
connaisse l'adresse d'un serveur DNS (pour résoudre des noms comme www.ynov.com afin de connaître l'adresse IP associée à ce nom)

Si l'un de ces points n'est pas correctement configuré, on est bien "connectés" (genre y'a un LAN, tout le monde se ping) mais sans "accès internet".


## 1. Accès internet routeur

Cette section 1. est à réaliser sur routeur.tp5.b1.

☀️ Déjà, prouvez que le routeur a un accès internet

une seule commande ping suffit à prouver ça, vers un nom de domaine que vous connaissez, genre www.ynov.com (ou autre de votre choix :d)
deso pour les faute mais le convertisseur de texte est vriament naze
```powershell
[root@routeur ~]# ping Ww.ynov.com
PING W.ynov.com (184.26.10.233) 56(84) bytes of data.
r ro64 bytes from 184.26.19.233 (184.26.18.233): icmp_seq=1 ttl-54 time -57.4 mS
64 bytes from 194.26.18.233 (184.26.18.233) : icmp_seq=2 ttl-54 time-28.8 ms
64 bytes from 194.26.18.233 (184.26.18.233) : icmp_seq=3 ttl-54 time-18.8 ms
est 64 bytes from 184.26.10.233 (184.26.18.233): icmp_seq=4 ttl-54 time-28.3 ms
^C
Md.ynov.com ping statist ics
4 packets transmitted, 4 received, 0% packet loss, time 3898ms
rtt min/avg/max/mdey = 17.988/29.135/57.435/16.374 ms
```

☀️ Activez le routage

toujours sur routeur.tp5.b1

la commande est dans le mémo toujours !


Tout est normalement déjà setup avec la carte NAT ! Si vous n'avez pas internet, c'est que votre carte NAT est éteinte. Allumez-la !

```powershell
[root@routeur ~]# sudo firewall-cmd --add-masquerade --permanent
success
[root@routeur ~]# sudo firewall-cmd --reload
success
[root@routeur ~]# ping www.ynov.com
PING www.ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=54 time=18.5 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=2 ttl=54 time=16.9 ms
^C 
--- www.ynov.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 16.948/17.741/18.535/0.793 ms
```
## 2. Accès internet clients

Cette section 2. est à réaliser sur client1.tp5.b1 et client2.tp5.b1. Tout est dans le mémo réseau Ubuntu.

➜ Définir l'adresse IP du routeur comme passerelle pour les clients

il sera peut-être nécessaire de redémarrer l'interface réseau pour que ça prenne effet

➜ Vérifier que les clients ont un accès internet

avec un ping vers une adresse IP publique vous connaissez
à ce stade, vos clients ne peuvent toujours pas résoudre des noms, donc impossible de visiter un site comme www.ynov.com


➜ Définir 1.1.1.1 comme serveur DNS que peuvent utiliser les clients

redémarrez l'interface réseau si nécessaire pour que ça prenne effet
ainsi vos clients pourront spontanément envoyer des requêtes DNS vers 1.1.1.1 afin d'apprendre à quelle IP correspond un nom de domaine donné


1.1.1.1 c'est l'adresse IP publique d'un serveur DNS d'une entreprise qui s'appelle CloudFlare (un gros acteur du Web). Ils hébergent gracieusement et publiquement ce serveur DNS, afin que n'importe qui puisse l'utiliser.

☀️ Prouvez que les clients ont un accès internet

avec de la résolution de noms cette fois
une seule commande ping suffit

```powershell
mael@client2:~/Desktop$ ping ynov.com
PING ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233: icmp_seq=1 ttl=53 time=17.5 ms
64 bytes from 104.26.11.233: icmp_seq=2 ttl=53 time=18.0 ms
64 bytes from 104.26.11.233: icmp_seq=3 ttl=53 time=21.3 ms
^C
--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2010ms
rtt min/avg/max/mdev = 17.467/18.941/21.328/1.703 ms

```

```powershell
mael@client1:~/Desktop$ ping ynov.com
PING ynov.com (104.26.10.233) 56(84) bytes of data.
64 bytes from 104.26.10.233: icmp_seq=1 ttl=53 time=17.7 ms
64 bytes from 104.26.10.233: icmp_seq=2 ttl=53 time=20.4 ms
64 bytes from 104.26.10.233: icmp_seq=3 ttl=53 time=18.7 ms
^C
--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 17.680/18.917/20.376/1.111 ms

```

☀️ Montrez-moi le contenu final du fichier de configuration de l'interface réseau

celui de client2.tp5.b1 me suffira
pour le compte-rendu, une simple commande cat pour afficher le contenu du fichier


Vous devriez pouvoir ouvrir un navigateur et visiter des sites sans soucis sur les clients.

```powershell
mael@client2:~/Desktop$ cat /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [10.5.1.12/24]
      gateway4: 10.5.1.254
      nameservers:
        addresses: [1.1.1.1,1.0.0.1]

```

# III. Serveur SSH

Cette partie III. est à réaliser sur routeur.tp5.b1. Tout est dans le mémo réseau Rocky.

☀️ Sur routeur.tp5.b1, déterminer sur quel port écoute le serveur SSH

pour le serveur SSH, le nom du programme c'est sshd

il écoute sur un port TCP


dans le compte rendu je veux que vous utilisiez une syntaxe avec ... | grep <PORT> pour isoler la ligne avec le port intéressant

par exemple si, vous repérez le port 8888, vous ajoutez  | grep 8888 à votre commande, pour me mettre en évidence le por que vous avez repéré

```powershell
[root@routeur ~]# ss -tuln | grep 22
State          Recv-Q         Send-Q         local Adress:Port       Peer Address:Port
LISTEN         0              128            0.0.0.0:22              0.0.0.:*
LISTEN         0              128            [::]:22                 [::]:*
```

☀️ Sur routeur.tp5.b1, vérifier que ce port est bien ouvert

la commande est dans le mémooooo pour voir la configuration du pare-feu


Si vous voyez le "service" ssh ouvert dans le pare-feu, il correspond à un port bien précis. Pour voir la correspondance entre les "services" et le port associé, vous pouvez consulter le contenu du fichier /etc/services. Ca devrait correspondre à ce que vous avez vu juste avant !

➜ Dernière fois que je le dis : connectez-vous en SSH pour administrer la machine Rocky Linux, n'utilisez pas l'interface console de VirtualBox.

```powershell
[mael@routeur ~]$ sudo firewall-cmd --list-all
[sudo] password for mael:
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

# IV. Serveur DHCP

## 1. Le but
➜ On installe et configure notre propre serveur DHCP dans cette partie ! Le but est le suivant :

dès qu'un client se connecte à notre réseau, il a automatiquement internet !
ça évite de faire à la main comme vous avez fait dans ce TP :

choisir et configurer une adresse IP
choisir et configurer l'adresse d'un serveur DNS
configurer l'adresse de la passerelle


dès qu'il se connecte, il essaiera automatiquement de contacter un serveur DHCP

notre serveur DHCP lui proposera alors automatiquement tout le nécessaire pour avoir un accès internet, à savoir :

une adresse IP disponible
l'adresse d'un serveur DNS
l'adresse de la passerelle du réseau





## 2. Comment le faire

Cette fois, je vous ré-écris pas tout, je vous laisse chercher sur internet par vous-mêmes "install dhcp server rocky 9", ou vous référer par exemple à ce lien qui résume très bien la chose.

➜ Peu importe le lien que vous suivez, les étapes seront les suivantes :

installation du paquet qui contient le serveur DHCP

commande dnf install



modification de la configuration

c'est un fichier texte (comme toujours)
donc avec nano ou vim par exemple


(re)démarrage du service DHCP

avec un systemctl start




➜ Et si ça fonctionne pas, c'est que tu t'es planté dans le fichier de conf, donc tu vas lire pourquoi dans les logs :

voir les logs d'erreur

avec une commande journalctl

généralement, il dit clairement l'erreur


ajustement de la configuration

c'est un fichier texte (comme toujours)
donc avec nano ou vim par exemple


redémarrage du service DHCP

avec un systemctl restart





N'hésitez pas, comme d'hab, à m'appeler si vous galérez avec cette section !


## 3. Rendu attendu

Vous pouvez éteindre client1.tp5.b1 et client2.tp5.b1 pour limiter l'utilisation des ressources hein.

⚠️⚠️⚠️ Vous n'avez le droit d'utiliser QUE des lignes que vous comprenez dans le fichier de configuration. Et pour lesquelles vous avez adapté les valeurs au TP. Vous devez enlever les lignes de configuration inutiles pour notre TP.

A. Installation et configuration du serveur DHCP

Cette section A. est à réaliser sur routeur.tp5.b1.

☀️ Installez et configurez un serveur DHCP sur la machine routeur.tp5.b1

je veux toutes les commandes réalisées
et le contenu du fichier de configuration
le fichier de configuration doit :

indiquer qu'on propose aux clients des adresses IP entre 10.5.1.137 et 10.5.1.237

indiquer aux clients que la passerelle dans le réseau ici c'est 10.5.1.254

indiquer aux clients qu'un serveur DNS joignable depuis le réseau c'est 1.1.1.1


```powershell
[mael@routeur ~]$ sudo nano /etc/dhcp/dhcpd.conf

# Nom de domaine
option domain-name "srv.world";

# Adresse du serveur DNS
option domain-name-servers 1.1.1.1;  # Utilisez le serveur DNS souhaité

# Temps de location par défaut
default-lease-time 600;

# Temps de location maximal
max-lease-time 7200;

# Déclaration de ce serveur DHCP comme valide
authoritative;

# Configuration du réseau et masque de sous-réseau
subnet 10.5.1.0 netmask 255.255.255.0 {
    # Plage d'adresses IP à attribuer
    range 10.5.1.137 10.5.1.237;
    
    # Adresse de diffusion
    option broadcast-address 10.5.1.255;  # Modifiez si nécessaire
    
    # Adresse de la passerelle
    option routers 10.5.1.254;
}


[mael@routeur ~]$ systemctl enable --now dhcpd
Failed to enable unit: Access denied
[mael@routeur ~]$ sudo systemctl enable --now dhcpd
[mael@routeur ~]$ sudo systemctl restart dhcpd
[mael@routeur ~]$ sudo systemctl enable dhcpd

[mael@routeur ~]$
[mael@routeur ~]$ sudo firewall-cmd --add-service=dhcp --permanent
md --reload
Warning: ALREADY_ENABLED: dhcp
success
[mael@routeur ~]$ sudo firewall-cmd --reload
success
[mael@routeur ~]$
```

B. Test avec un nouveau client

Cette section B. est à réaliser sur une nouvelle machine Ubuntu fraîchement clonée : client3.tp5.b1. Vous pouvez éteindre client1.tp5.b1 et client2.tp5.b1 pour économiser des ressources.

☀️ Créez une nouvelle machine client client3.tp5.b1

définissez son hostname

définissez une IP en DHCP
vérifiez que c'est bien une adresse IP entre .137 et .237

prouvez qu'il a immédiatement un accès internet

```powershell
mael@mael-VirtualBox:~/Desktop$ hostnamectl
 Static hostname: client3
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 25d95201e2c24e3bbe983cb99358dbe2
         Boot ID: 667803396c354428a51c0e734e2a16e9
  Virtualization: oracle
Operating System: Ubuntu 24.04.1 LTS              
          Kernel: Linux 6.8.0-45-generic
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Firmware Version: VirtualBox
   Firmware Date: Fri 2006-12-01
    Firmware Age: 17y 10month 2w 1d   
mael@mael-VirtualBox:~/Desktop$ sudo nano /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
mael@mael-VirtualBox:~/Desktop$ sudo netplan apply

** (generate:6187): WARNING **: 12:17:19.197: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:6186): WARNING **: 12:17:19.475: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:6186): WARNING **: 12:17:19.566: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.
mael@mael-VirtualBox:~/Desktop$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0e:b4:ef brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 561sec preferred_lft 561sec
mael@mael-VirtualBox:~/Desktop$ ping www.ynov.com
PING www.ynov.com (104.26.10.233) 56(84) bytes of data.
64 bytes from 104.26.10.233: icmp_seq=1 ttl=53 time=18.3 ms
64 bytes from 104.26.10.233: icmp_seq=2 ttl=53 time=21.3 ms
64 bytes from 104.26.10.233: icmp_seq=3 ttl=53 time=19.2 ms
^C
--- www.ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2018ms
rtt min/avg/max/mdev = 18.322/19.599/21.323/1.265 ms

```

C. Consulter le bail DHCP
➜ Côté serveur DHCP, à chaque fois qu'une adresse IP est proposée à quelqu'un, le serveur crée un fichier texte appelé bail DHCP (ou DHCP lease en anglais).
Il contient toutes les informations liées à l'échange avec le client, notamment :

adresse MAC du client qui a demandé l'IP
adresse IP proposée au client
heure et date précises de l'échange DHCP
durée de validité du bail DHCP



A l'issue de cette durée de validité, le client devra de nouveau contacter le serveur, pour s'assurer que l'adresse IP est toujours libre. Rappelez-vous que DHCP est utilisé partout pour attribuer automatiquement des adresses IP aux clients, à l'école, chez vous, etc. C'est nécessaire que le bail expire pour pas qu'un client qui se connecte une seule fois monopolise à vie une adresse IP.

☀️ Consultez le bail DHCP qui a été créé pour notre client

à faire sur routeur.tp5.b1

toutes les données du serveur DHCP, comme les baux DHCP, sont stockés dans le dossier /var/lib/dhcpd/

afficher le contenu du fichier qui contient les baux DHCP

on devrait y voir l'IP qui a été proposée au client, ainsi que son adresse MAC

```powershell
[mael@routeur dhcpd]$ sudo cat dhcpd.leases
# The format of this file is documented in the dhcpd.leases(5) manual page.
# This lease file was written by isc-dhcp-4.4.2b1

# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

server-duid "\000\001\000\001.\242FB\010\000'\3725\272";

lease 10.5.1.137 {
  starts 3 2024/10/16 10:12:39;
  ends 3 2024/10/16 10:22:39;
  cltt 3 2024/10/16 10:12:39;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:0e:b4:ef;
  uid "\377\3424?>\000\002\000\000\253\021a\201\251\252\350\277\325\012";
  client-hostname "client3";
}
lease 10.5.1.137 {
  starts 3 2024/10/16 10:12:39;
  ends 3 2024/10/16 10:17:21;
  tstp 3 2024/10/16 10:17:21;
  cltt 3 2024/10/16 10:12:39;
  binding state free;
  hardware ethernet 08:00:27:0e:b4:ef;
  uid "\377\3424?>\000\002\000\000\253\021a\201\251\252\350\277\325\012";
}
lease 10.5.1.137 {
  starts 3 2024/10/16 10:17:25;
  ends 3 2024/10/16 10:27:25;
  cltt 3 2024/10/16 10:17:25;
  binding state active;
  next binding state free;
  rewind binding state free;
  ☀️hardware ethernet 08:00:27:0e:b4:ef;
  uid "\377\3424?>\000\002\000\000\253\021a\201\251\252\350\277\325\012";
  client-hostname "client3";
}
```

☀️ Confirmez qu'il s'agit bien de la bonne adresse MAC

à faire sur client3.tp5.b1

consultez l'adresse MAC du client
on peut consulter les adresses MAC des cartes réseau avec un simple ip a

```powershell
mael@mael-VirtualBox:~/Desktop$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    ☀️link/ether 08:00:27:0e:b4:ef brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 498sec preferred_lft 498sec

```