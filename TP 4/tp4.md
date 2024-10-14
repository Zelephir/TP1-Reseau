# 1. Les mains dans le capot
## ☀️ Capturez un échange DHCP complet

capture dhcp.pcap

il y a 4 trames (le DORA) : Discover, Offer, Request, Acknowledge


Soucis : l'échange DHCP complet ne se produit qu'à la première connexion. Pour forcer un échange DHCP, ça dépend de votre OS. Sur GNU/Linux, avec dhclient ça se fait bien. Sur Windows, le plus simple reste de définir une IP statique pourrie sur la carte réseau, se déconnecter du réseau, remettre en DHCP, se reconnecter au réseau (ché moa sa march). Essayez de regarder par vous-mêmes s'il existe une commande clean pour faire ça ! Sur MacOS, je connais peu mais Internet dit qu'c'est po si compliqué, appelez moi si besoin.

[c lo](dhcp.pcap)

## ☀️ Directement dans Wireshark, vous pouvez voir toutes les infos que vous donne  le serveur DHCP

retrouvez dans l'échange DHCP les 3 infos dont on parle plus haut :

adresse IP proposée
serveur DNS indiqué
passerelle du réseau



pour le compte-rendu le nom des champs et leur valeur, pour ces 3 valeurs

par exemple (l'exemple te parlera + quand t'auras regardé dans Wireshark) :

Option(1337) Meow: 10.10.10.10

```powershell
Your (client) IP address: 192.168.1.108
Option: (6) Domain Name Server
    - Domain Name Server: 192.168.1.10
Option: (3) Router
    - Router: 192.168.1.1
```