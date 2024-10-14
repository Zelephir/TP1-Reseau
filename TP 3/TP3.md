# I. ARP basics

⚠️ Pas besoin du partage de co pour le moment, restez sur le réseau de l'école en WiFi.

## ☀️ Avant de continuer...

affichez l'adresse MAC de votre carte WiFi !

```powershell
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek MT7921 Wi-Fi 6 802.11ax PCIe Adapter
   Adresse physique . . . . . . . . . . . : 34-6F-24-E2-DA-27
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::d31a:da20:d46:3687%12(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.78.234(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : mardi 8 octobre 2024 12:31:55
   Bail expirant. . . . . . . . . . . . . : mercredi 9 octobre 2024 08:16:44
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 137654052
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-BE-AB-18-5C-60-BA-E4-58-AB
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

## ☀️ Affichez votre table ARP

allez vous commencez à devenir grands, je vous donne pas la commande, demande à m'sieur internet !

```powershell
PS C:\WINDOWS\system32> arp -a

Interface : 10.33.78.234 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  234.78.33.10          01-00-5e-4e-21-0a     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  234.78.33.10          01-00-5e-4e-21-0a     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

## ☀️ Déterminez l'adresse MAC de la passerelle du réseau de l'école

la passerelle, vous connaissez son adresse IP normalement (cf TP1 ;) )
si vous avez un accès internet, votre PC a forcément l'adresse MAC de la passerelle dans sa table ARP

```powershell
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```

## ☀️ Supprimez la ligne qui concerne la passerelle

une commande pour supprimer l'adresse MAC de votre table ARP
si vous ré-affichez votre table ARP, y'a des chances que ça revienne presque tout de suite !
```powershell
arp -d 10.33.79.254
```

## ☀️ Prouvez que vous avez supprimé la ligne dans la table ARP

en affichant la table ARP
si la ligne est déjà revenue, déconnecte-toi temporairement du réseau de l'école, et supprime-la de nouveau

```powershell
PS C:\WINDOWS\system32> arp -a

Interface : 10.33.78.234 --- 0xc
  Adresse Internet      Adresse physique      Type
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  234.78.33.10          01-00-5e-4e-21-0a     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  234.78.33.10          01-00-5e-4e-21-0a     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

## ☀️ Wireshark

capture arp1.pcap

lancez une capture Wireshark, puis supprimez la ligne de la passerelle dans la table ARP pendant que la capture est en cours
la capture doit contenir uniquement 2 trames :

un ARP request que votre PC envoie pour apprendre l'adresse MAC de la passerelle
et la réponse


Si vous les voyez pas, ça veut dire que votre PC n'utilise pas du tout de connexion vers l'extérieur, genre vers internet. Ce qui paraît invraisemblable on est d'accord non ? Vous avez vu ce que ça fait avec netstat aux TPs précédents quand on affiche toutes les connexions réseau en cours ! Très peu de chance que votre PC ne contacte pas internet toutes les quelques secondes.

[c lo](.\)

II. ARP dans un réseau local

⚠️ Avant de continuer, regroupez-vous par 3 ou 4, et connectez-vous au même partage de co avec le téléphone de l'un d'entre vous. Si ça fonctionne pas, vous pouvez faire cette partie sur le réseau de l'école mais moins bieng.

Dans cette situation, le téléphone agit comme une "box" :


c'est un switch

il permet à tout le monde d'être connecté à un même réseau local



c'est aussi un routeur

il fait passer les paquets du réseau local, à internet
et vice-versa
on dit que ce routeur est la passerelle du réseau local




1. Basics

⚠️ Avant de continuer, regroupez-vous par 3 ou 4, et connectez-vous au même partage de co avec le téléphone de l'un d'entre vous. Si ça fonctionne pas, vous pouvez faire cette partie sur le réseau de l'école mais moins bieng.

## ☀️ Déterminer

pour la carte réseau impliquée dans le partage de connexion (carte WiFi ?)
son adresse IP au sein du réseau local formé par le partage de co
son adresse MAC

```powershell
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek MT7921 Wi-Fi 6 802.11ax PCIe Adapter
   Adresse physique . . . . . . . . . . . : 34-6F-24-E2-DA-27
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6. . . . . . . . . . . . . .: 2a01:cb01:3036:fade:fdb3:bfdf:4726:9048(préféré)
   Adresse IPv6 temporaire . . . . . . . .: 2a01:cb01:3036:fade:c171:454c:d2c0:2700(préféré)
   Adresse IPv6 de liaison locale. . . . .: fe80::d31a:da20:d46:3687%12(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 172.20.10.5(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.240
   Bail obtenu. . . . . . . . . . . . . . : mercredi 9 octobre 2024 13:31:16
   Bail expirant. . . . . . . . . . . . . : jeudi 10 octobre 2024 13:31:17
   Passerelle par défaut. . . . . . . . . : fe80::94b0:1fff:fe27:6964%12
                                       172.20.10.1
   Serveur DHCP . . . . . . . . . . . . . : 172.20.10.1
   IAID DHCPv6 . . . . . . . . . . . : 137654052
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-BE-AB-18-5C-60-BA-E4-58-AB
   Serveurs DNS. . .  . . . . . . . . . . : fe80::94b0:1fff:fe27:6964%12
                                       172.20.10.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

## ☀️ DIY

changer d'adresse IP

vous pouvez le faire en interface graphique


faut procéder comme au TP1 :

vous respectez les informations que vous connaissez du réseau
remettez la même passerelle, le même masque, et le même serveur DNS
changez seulement votre adresse IP, ne changez que le dernier nombre


prouvez que vous avez bien changé d'IP

avec une commande !

```powershell
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek MT7921 Wi-Fi 6 802.11ax PCIe Adapter
   Adresse physique . . . . . . . . . . . : 34-6F-24-E2-DA-27
   DHCP activé. . . . . . . . . . . . . . : Non
   Configuration automatique activée. . . : Oui
   Adresse IPv6. . . . . . . . . . . . . .: 2a01:cb01:3036:fade:fdb3:bfdf:4726:9048(préféré)
   Adresse IPv6 temporaire . . . . . . . .: 2a01:cb01:3036:fade:7867:b879:c904:1b8d(préféré)
   Adresse IPv6 de liaison locale. . . . .: fe80::d31a:da20:d46:3687%12(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 172.20.10.9(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.240
   Passerelle par défaut. . . . . . . . . : fe80::94b0:1fff:fe27:6964%12
   IAID DHCPv6 . . . . . . . . . . . : 137654052
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-BE-AB-18-5C-60-BA-E4-58-AB
   Serveurs DNS. . .  . . . . . . . . . . : fe80::94b0:1fff:fe27:6964%12
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

## ☀️ Pingz !

vérifiez que vous pouvez tous vous ping avec ces adresses IP
vérifiez avec une commande ping que vous avez bien un accès internet


Pour vérifier l'accès internet avec un ping il suffit d'envoyer un ping vers une adresse IP publique ou un nom de domaine public que vous connaissez. Genre ping xkcd.com, ça fonctionne, peu importe, du moment que c'est sur internet.

```powershell
PS C:\WINDOWS\system32> ping -t 172.20.10.3

Envoi d’une requête 'Ping'  172.20.10.3 avec 32 octets de données :
Réponse de 172.20.10.3 : octets=32 temps=2034 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=121 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=34 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=140 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=28 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=152 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=66 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=89 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=117 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=104 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=106 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=121 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=132 ms TTL=128

Statistiques Ping pour 172.20.10.3:
    Paquets : envoyés = 13, reçus = 13, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 28ms, Maximum = 2034ms, Moyenne = 249ms
Ctrl+C
```

# 2. ARP
## ☀️ Affichez votre table ARP !

normalement, après les ping, vous avez appris l'adresse MAC de tous les autres

➜ Wireshark that

lancez tous Wireshark
videz tous vos tables ARP
normalement, y'a presque pas de raisons que vos PCs se contactent entre eux spontanément donc les tables ARP devraient rester vides tant que vous faites rien
à tour de rôle, envoyez quelques ping entre vous
constatez les messages ARP spontanés qui précèdent vos ping

ARP request

envoyé en broadcast ff:ff:ff:ff:ff:ff

tout le monde les reçoit donc !


ARP reply

celui qui a été ping répond à celui qui a initié le `pingv
il l'informe que l'adresse IP qui a été ping correspond à son adresse MAC

[c lo](pingco.pcap)

## ☀️ Capture arp2.pcap

[c lo](arp1.pcap)

ne contient que des ARP request et ARP reply
contient tout le nécessaire pour que votre table ARP soit populée avec l'adresse MAC de tout le monde