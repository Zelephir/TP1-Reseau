# TP6 : Des bo services dans des bo LANs

## 2. Marche à suivre
➜ Créez deux nouveaux host-only

attribuez à votre PC les adresses IP indiquées dans le tableau d'adressage

➜ Créez toutes les machines virtuelles

clones dans tous les sens
Ubuntu (ou OS de votre choix) pour client1.tp6.b1

Rocky Linux pour tous les autres
branchez-les aux bons host-only :D

➜ Allumez tout et attribuez les adresses IP indiquées

sauf client1.tp6.b1, no need pour le moment
configuration réseau sur toutes les machines :

adresse IP statique sur les cartes host-only
indiquez l'adresse IP de votre routeur comme passerelle

laquelle ? Celle qui est dans ton LAN !
par exemple, une machine du LAN1, bah sa passerelle c'est 10.6.1.254 (et PAS 10.6.2.254)


indiquez 1.1.1.1 comme DNS


n'oubliez pas d'activer le routage sur routeur.tp6.b1


NOMMEZ VOS MACHINES (configuration du hostname, voir mémo)

☀️ Prouvez que...

une machine du LAN1 peut joindre internet (ping un nom de domaine)

```powershell
[mael@dhcp ~]$ ping www.google.com
PING www.google.com (142.250.201.36) 56(84) bytes of data.
64 bytes from mrs08s20-in-f4.1e100.net (142.250.201.36): icmp_seq=1 ttl=112 time=21.3 ms
64 bytes from mrs08s20-in-f4.1e100.net (142.250.201.36): icmp_seq=2 ttl=112 time=20.1 ms
64 bytes from mrs08s20-in-f4.1e100.net (142.250.201.36): icmp_seq=3 ttl=112 time=21.0 ms
64 bytes from mrs08s20-in-f4.1e100.net (142.250.201.36): icmp_seq=4 ttl=112 time=21.6 ms
64 bytes from mrs08s20-in-f4.1e100.net (142.250.201.36): icmp_seq=5 ttl=112 time=21.6 ms
^C
--- www.google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4011ms
rtt min/avg/max/mdev = 20.081/21.100/21.623/0.556 ms
```

une machine du LAN2 peut joindre internet (ping nom de domaine)

```powershell
[mael@dns ~]$ ping ynov.com
PING ynov.com (172.67.74.226) 56(84) bytes of data.
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=1 ttl=53 time=20.6 ms
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=2 ttl=53 time=20.3 ms
64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=3 ttl=53 time=24.2 ms
^C64 bytes from 172.67.74.226: icmp_seq=4 ttl=53 time=79.8 ms

--- ynov.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3008ms
rtt min/avg/max/mdev = 20.345/36.212/79.788/25.203 ms
```

une machine du LAN1 peut joindre une machine du LAN2 (ping une adresse IP)

```powershell
[mael@dns ~]$ ping 10.6.1.253
PING 10.6.1.253 (10.6.1.253) 56(84) bytes of data.
64 bytes from 10.6.1.253: icmp_seq=1 ttl=63 time=4.44 ms
64 bytes from 10.6.1.253: icmp_seq=2 ttl=63 time=2.54 ms
64 bytes from 10.6.1.253: icmp_seq=3 ttl=63 time=3.32 ms
^C
--- 10.6.1.253 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2007ms
rtt min/avg/max/mdev = 2.543/3.434/4.441/0.779 ms
```

Prouvez que tout fonctionne quoi ! :d Je veux voir le nom des machines dans le prompt, vous devez donc avoir nommé vos machines.


# II. LAN clients
Partie dédiée à la configuration du LAN1 : 10.6.1.0/24. Un réseau dans lequel des clients pourraient se connecter, accéder à internet, et plus tard accéder à nos services internes (un ptit site web de fou, ce sera partie III).

## 1. Serveur DHCP

A faire sur dhcp.tp6.b1.

Dans cette section, install + config du serveur DHCP. Pour que tous les clients du LAN1 puisse avoir un accès au réseau dès leur connexion !
➜ Installez et configurez un serveur DHCP sur dhcp.tp6.b1

pareil qu'au TP5
il doit attribuer des IPs entre .37 et .137

il doit indiquer la bonne passerelle pour les clients
il doit indiquer 1.1.1.1 comme serveur DNS aux clients

## 2. Client

A faire sur client1.tp6.b1.

➜ Allumez client1.tp6.b1 et configurez sa carte réseau en DHCP

il devrait récupérer automatiquement une adresse IP auprès de votre serveur DHCP
et apprendre l'adresse de la passerelle de ce réseau
et l'adresse d'un DNS utilisable

☀️ Prouvez que...

le client a bien récupéré une adresse IP en DHCP

avec un ip a le mot-clé dynamic doit être écrit sur la ligne qui contient l'adresse IP
(y'a pas ce mot-clé quand tu définis une IP statique)


vous avez bien 1.1.1.1 en DNS

commande dans le mémo pour consulter l'adresse IP du serveur DNS connu


vous avez bien la bonne passerelle indiquée

idem, dans le mémo, pour afficher l'adresse de la passerelle actuellement configurée :)


que ça ping un nom de domaine public sans problème magueule

```powershell
mael@client3:~/Desktop$ sudo nano /etc/netplan/01-netcfg.yaml
[sudo] password for mael: 

mael@client3:~/Desktop$ sudo netplan apply

** (generate:3777): WARNING **: 12:26:52.202: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:3776): WARNING **: 12:26:52.529: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:3776): WARNING **: 12:26:52.623: Permissions for /etc/netplan/01-netcfg.yaml are too open. Netplan configuration should NOT be accessible by others.

mael@client3:~/Desktop$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:02:ef:f4 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.37/24 metric 100 brd 10.6.1.255 scope global dynamic enp0s3
       valid_lft 599sec preferred_lft 599sec
    inet6 fe80::a00:27ff:fe02:eff4/64 scope link 
       valid_lft forever preferred_lft forever
3: mpqemubr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:a3:35:7f brd ff:ff:ff:ff:ff:ff
    inet 10.237.56.1/24 brd 10.237.56.255 scope global mpqemubr0
       valid_lft forever preferred_lft forever

mael@client3:~/Desktop$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
       DNS Servers: 1.1.1.1
        DNS Domain: srv.world

Link 3 (mpqemubr0)
    Current Scopes: none
         Protocols: -DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported

mael@client3:~/Desktop$ ping ynov.com
PING ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233: icmp_seq=1 ttl=53 time=25.7 ms
64 bytes from 104.26.11.233: icmp_seq=2 ttl=53 time=21.1 ms
^C
--- ynov.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1056ms
rtt min/avg/max/mdev = 21.115/23.389/25.664/2.274 ms

mael@client3:~/Desktop$ ip route show
default via 10.6.1.254 dev enp0s3 proto dhcp src 10.6.1.37 metric 100 
1.1.1.1 via 10.6.1.254 dev enp0s3 proto dhcp src 10.6.1.37 metric 100 
10.6.1.0/24 dev enp0s3 proto kernel scope link src 10.6.1.37 metric 100 
10.6.1.254 dev enp0s3 proto dhcp scope link src 10.6.1.37 metric 100 
10.237.56.0/24 dev mpqemubr0 proto kernel scope link src 10.237.56.1 linkdown 

mael@client3:~/Desktop$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=19.5 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=53 time=22.1 ms
^C
--- 1.1.1.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 19.472/20.776/22.080/1.304 ms
```
# III. LAN serveurzzzz
Troisième partie, dédiée à la configuration de nos deux serveurs. Pour l'instant c'est juste des Rocky sans âme :d

## 1. Serveur Web

A faire sur web.tp6.b1.

# III. 1. Serveur Web

1. Intro serveur Web
2. Install this shiet
3. Analyse et test




1. Intro serveur Web
On appelle serveur Web une machine qui exécute un programme qu'on appelle un service Web.
Un service Web est un programme qui écoute sur un port, attend la connexion de clients.

Par convention, le port sur lequel écoute les services Web HTTP c'est 80/tcp.

Un client est supposé se connecter au port envoyer une requête HTTP, et le serveur renverra alors une page HTML qui correspond à la requête du client.

Comme nc aux premiers TPs, sauf que quand tu te connectes, et que t'envoies la bonne requête, bah le service Web répond automatiquement une page HTML.

On va utiliser un service Web qui s'appelle NGINX, réputé pour ses performances et sa stabilité !

2. Install this shiet

On est sur notre (futur) beau serveur Web web.tp6.b1.

On est pas en cours de Linux, ni en cours de dév Web, alors on va s'intéresser à la partie réseau uniquement, et limiter au maximum les interactions spécifiques à Linux.
➜ Installez et démarrez le service web NGINX

# installation du service NGINX
sudo dnf install -y nginx

# démarrage du service NGINX
sudo systemctl enable --now nginx
Devrait po y avoir d'erreurs, et ça tourne !

Le service web NGINX propose une page d'accueil (toute moche) par défaut. On aura même pas besoin d'écrire une vieille page HTML pour tester :D Il permet si on le configure d'héberger les sites Web de notre choix dans une utilisation réelle.


☀️ Déterminer sur quel port écoute le serveur NGINX

commande dans le mémooooooo
isolez la ligne intéressante avec un ... | grep <PORT> une fois que vous avez repéré le port

```powershell
[mael@web ~]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=5473,fd=6),("nginx",pid=5472,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=5473,fd=7),("nginx",pid=5472,fd=7))
```

C'est le port conventionnel ! Pour un service Web, qui reçoit donc des requêtes HTTP, c'est ce port qui est la convention :) C'est aussi écrit dans la configuration quelque port qu'il doit écouter sur ce port. Conf dans le dossier /etc/nginx/ pour les curieux.

☀️ Ouvrir ce port dans le firewall

bien que NGINX soit en train d'écouter sur le port repéré, et attende la connexion de clients potentiels...
... ça empêche pas le firewall de bloquer les clients si on ouvre pas le port
commande dans le mémooooo pour faire ça aussi

```powershell
[mael@web ~]$ sudo firewall-cmd --permanent --add-port=80/tcp
success
[mael@web ~]$ sudo firewall-cmd --reload
success
```

☀️ Visitez le site web !

depuis client1.tp6.b1, ouvrez un navigateur, et visitez http://10.6.2.11 (l'adresse IP du serveur web web.tp6.b1)
normalement, page d'accueil !

sinon, tu t'es planté, vérifie tout !


pour le compte-rendu, depuis le terminal de client1.tp6.b1 : curl http://10.6.2.11

vous me mettez que les premières lignes du résultat SVP :D
la commande curl envoie une requête HTTP (comme un navigateur), en réponse on tout l'HTML de la page... bonne bouillie :D

```powershell
mael@client3:~/Desktop$ curl http://10.6.2.11
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/
      
      html {
        height: 100%;

```

## 2. Serveur DNS

A faire sur dns.tp6.b1.

Document dédié au setup serveur DNS

## 4. Analyse du service
☀️ Déterminer sur quel(s) port(s) écoute le service BIND9

isolez la ligne intéressante avec un ... | grep <PORT> une fois que vous avez repéré le port
bon normalement vous l'avez vu dans la conf, non ? :d

```powershell
[mael@dns ~]$ sudo ss -lnpt | grep 53
LISTEN 0      4096       127.0.0.1:953       0.0.0.0:*    users:(("named",pid=11603,fd=28))
LISTEN 0      10         10.6.2.12:53        0.0.0.0:*    users:(("named",pid=11603,fd=25))
LISTEN 0      10         127.0.0.1:53        0.0.0.0:*    users:(("named",pid=11603,fd=22))
LISTEN 0      4096           [::1]:953          [::]:*    users:(("named",pid=11603,fd=29))
LISTEN 0      10             [::1]:53           [::]:*    users:(("named",pid=11603,fd=27))
```

☀️ Ouvrir ce(s) port(s) dans le firewall

```powershell
[mael@dns ~]$ sudo firewall-cmd --permanent --add-port=53/tcp
success
[mael@dns ~]$ sudo firewall-cmd --reload
success
```

## 5. Tests manuels
☀️ Effectuez des requêtes DNS manuellement depuis le serveur DNS lui-même dans un premier temps

```powershell
[mael@dns ~]$ dig web.tp6.b1 @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63021
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 7c5ff46ed845160b01000000671a07b50bdb88d723890895 (good)
;; QUESTION SECTION:
;web.tp6.b1.                    IN      A

;; ANSWER SECTION:
web.tp6.b1.             86400   IN      A       10.6.2.11

;; Query time: 6 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Thu Oct 24 10:39:17 CEST 2024
;; MSG SIZE  rcvd: 83

[mael@dns ~]$ dig dns.tp6.b1 @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> dns.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 37503
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: ce8f29a42cc2ace001000000671a07c5dab7b61560afec8e (good)
;; QUESTION SECTION:
;dns.tp6.b1.                    IN      A

;; ANSWER SECTION:
dns.tp6.b1.             86400   IN      A       10.6.2.12

;; Query time: 0 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Thu Oct 24 10:39:33 CEST 2024
;; MSG SIZE  rcvd: 83

[mael@dns ~]$ dig ynov.com @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> ynov.com @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 25070
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 8c4c423af388e35101000000671a07cb7464809464055b3b (good)
;; QUESTION SECTION:
;ynov.com.                      IN      A

;; ANSWER SECTION:
ynov.com.               300     IN      A       172.67.74.226
ynov.com.               300     IN      A       104.26.10.233
ynov.com.               300     IN      A       104.26.11.233

;; Query time: 245 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Thu Oct 24 10:39:39 CEST 2024
;; MSG SIZE  rcvd: 113

[mael@dns ~]$ dig -x 10.6.2.11 @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.11 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56627
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 05c22eb642d255d001000000671a07d5c82d0c1c1726dbef (good)
;; QUESTION SECTION:
;11.2.6.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
11.2.6.10.in-addr.arpa. 86400   IN      PTR     web.tp6.b1.

;; Query time: 13 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Thu Oct 24 10:39:49 CEST 2024
;; MSG SIZE  rcvd: 103

[mael@dns ~]$ dig -x 10.6.2.12 @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.12 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12546
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: f5625a313488933d01000000671a07dafb758708b50f6061 (good)
;; QUESTION SECTION:
;12.2.6.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
12.2.6.10.in-addr.arpa. 86400   IN      PTR     dns.tp6.b1.

;; Query time: 0 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Thu Oct 24 10:39:54 CEST 2024
;; MSG SIZE  rcvd: 103
```

☀️ Effectuez une requête DNS manuellement depuis client1.tp6.b1

pour obtenir l'adresse IP qui correspond au nom web.tp6.b1

```powershell
mael@client3:~/Desktop$ dig web.tp6.b1

; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> web.tp6.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 9989
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;web.tp6.b1.			IN	A

;; AUTHORITY SECTION:
.			86400	IN	SOA	a.root-servers.net. nstld.verisign-grs.com. 2024102400 1800 900 604800 86400

;; Query time: 39 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Oct 24 10:48:15 CEST 2024
;; MSG SIZE  rcvd: 114

```

☀️ Capturez une requête DNS et la réponse de votre serveur

vous pouvez télécharger Wireshark sur client1.tp6.b1 mais bon... bourrin
sinon vous pouvez lancer une capture réseau avec tcpdump, on pourra ensuite ouvrir le fichier avec Wireshark (sur votre PC, pas dans une VM) pour visualiser

```powershell
mael@client3:~/Desktop$ sudo tcpdump -w dns_capture.pcap -i enp0s3 port 53
tcpdump: listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
^C31 packets captured
31 packets received by filter
0 packets dropped by kernel
mael@client3:~/Desktop$ ^C
mael@client3:~/Desktop$ tcpdump -r dns_capture.pcap
reading from file dns_capture.pcap, link-type EN10MB (Ethernet), snapshot length 262144
10:54:31.348354 IP client3.44321 > 10.6.2.12.domain: 65456+ [1au] A? web.tp6.b1. (51)
10:54:31.352243 IP client3.43912 > 10.6.2.12.domain: 65456+ [1au] A? web.tp6.b1. (51)
10:54:31.356764 IP client3.59506 > 10.6.2.12.domain: 65456+ [1au] A? web.tp6.b1. (51)
10:56:01.847029 IP client3.47808 > one.one.one.one.domain: 14247+ [1au] A? cdimage.ubuntu.com. (47)
10:56:01.847673 IP client3.36315 > one.one.one.one.domain: 46091+ [1au] AAAA? cdimage.ubuntu.com. (47)
10:56:01.859904 IP client3.56292 > one.one.one.one.domain: 62804+ [1au] A? cloud-images.ubuntu.com. (52)
10:56:01.863372 IP client3.56217 > one.one.one.one.domain: 21630+ [1au] AAAA? cloud-images.ubuntu.com. (52)
10:56:01.871223 IP one.one.one.one.domain > client3.47808: 14247 3/0/1 A 91.189.91.124, A 91.189.91.123, A 185.125.190.37 (95)
10:56:01.881995 IP one.one.one.one.domain > client3.56292: 62804 2/0/1 A 185.125.190.37, A 185.125.190.40 (84)
10:56:01.888968 IP one.one.one.one.domain > client3.36315: 46091 3/0/1 AAAA 2001:67c:1562::28, AAAA 2001:67c:1562::25, AAAA 2620:2d:4000:1::17 (131)
10:56:01.976502 IP one.one.one.one.domain > client3.56217: 21630 2/0/1 AAAA 2620:2d:4000:1::17, AAAA 2620:2d:4000:1::1a (108)
10:56:14.155968 IP client3.54219 > one.one.one.one.domain: 27182+ [1au] A? contile.services.mozilla.com. (57)
10:56:14.158575 IP client3.44554 > one.one.one.one.domain: 65088+ [1au] HTTPS? contile.services.mozilla.com. (57)
10:56:14.162819 IP client3.46100 > one.one.one.one.domain: 49107+ [1au] AAAA? contile.services.mozilla.com. (57)
10:56:14.176523 IP one.one.one.one.domain > client3.54219: 27182 1/0/1 A 34.117.188.166 (73)
10:56:14.179970 IP one.one.one.one.domain > client3.44554: 65088 0/1/1 (138)
10:56:14.183825 IP one.one.one.one.domain > client3.46100: 49107 0/1/1 (138)
10:56:16.649795 IP client3.55274 > one.one.one.one.domain: 35598+ [1au] HTTPS? temuaffiliateprogram.pxf.io. (56)
10:56:16.656254 IP client3.41585 > one.one.one.one.domain: 43950+ [1au] A? temuaffiliateprogram.pxf.io. (56)
10:56:16.666569 IP client3.59685 > one.one.one.one.domain: 27420+ [1au] AAAA? temuaffiliateprogram.pxf.io. (56)
10:56:16.666940 IP client3.37346 > one.one.one.one.domain: 21467+ [1au] A? fr.hotels.com. (42)
10:56:16.676029 IP one.one.one.one.domain > client3.55274: 35598 0/1/1 (140)
10:56:16.680752 IP client3.48596 > one.one.one.one.domain: 32618+ [1au] AAAA? fr.hotels.com. (42)
10:56:16.704634 IP one.one.one.one.domain > client3.41585: 43950 1/0/1 A 35.201.76.231 (72)
10:56:16.704635 IP one.one.one.one.domain > client3.59685: 27420 0/1/1 (140)
10:56:16.704635 IP one.one.one.one.domain > client3.37346: 21467 3/0/1 CNAME ipv6-global.hotels.com.edgekey.net., CNAME e10109.dscx.akamaiedge.net., A 23.203.162.75 (143)
10:56:16.704635 IP one.one.one.one.domain > client3.48596: 32618 4/0/1 CNAME ipv6-global.hotels.com.edgekey.net., CNAME e10109.dscx.akamaiedge.net., AAAA 2a02:26f0:b80:693::277d, AAAA 2a02:26f0:b80:69d::277d (183)
10:56:16.735090 IP client3.55314 > one.one.one.one.domain: 9858+ [1au] HTTPS? e10109.dscx.akamaiedge.net. (55)
10:56:16.762626 IP one.one.one.one.domain > client3.55314: 9858 0/1/1 (119)
10:56:50.875477 IP client3.60575 > 10.6.2.12.domain: 33523+ [1au] A? web.tp6.b1. (51)
10:56:50.880533 IP 10.6.2.12.domain > client3.60575: 33523* 1/0/1 A 10.6.2.11 (83)

```

## 3. Serveur DHCP
What ?! Again ?!
Nan nan juste, on va aller changer une ligne dans la configuration de dhcp.tp6.b1.
Bah oui ! On a notre propre serveur DNS maintenant ! Faut le dire à nos clients qu'ils peuvent l'utiliser :d
➜ Editez la configuration du serveur DHCP sur dhcp.tp6.b1

faut qu'il file l'adresse IP 10.6.2.12 comme DNS à tous les nouveaux clients !

☀️ Créez un nouveau client client2.tp6.b1 vitefé

supprimez le client1 si vous voulez, on a pu besoin
récupérez une IP en DHCP sur ce nouveau client2.tp6.b1

vérifiez que vous avez bien 10.6.2.12 comme serveur DNS à contacter

commande dans le mémo pour voir le serveur DNS connu actuellement



➜ Vous devriez pouvoir visiter http://web.tp6.b1 avec le navigateur, ça devrait fonctionner sans aucune autre action.

```powershell
mael@mael-VirtualBox:~/Desktop$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
       DNS Servers: 10.6.2.12
        DNS Domain: srv.world

```
