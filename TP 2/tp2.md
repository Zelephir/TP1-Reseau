# 1. Quelques pings
🌞 Prouvez que votre configuration est effective

```powershell
PS C:\WINDOWS\system32> Get-NetIPAddress -InterfaceAlias "Ethernet"
>>


IPAddress         : fe80::2a2f:7fc9:4d9b:a3fd%15
InterfaceIndex    : 15
InterfaceAlias    : Ethernet
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.111.222.202
InterfaceIndex    : 15
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore
```
🌞 Tester que votre LAN + votre adressage IP est fonctionnel

```powershell
PS C:\WINDOWS\system32> ping 10.111.222.223

Envoi d’une requête 'Ping'  10.111.222.223 avec 32 octets de données :
Réponse de 10.111.222.223 : octets=32 temps=4 ms TTL=128
Réponse de 10.111.222.223 : octets=32 temps=4 ms TTL=128
Réponse de 10.111.222.223 : octets=32 temps=2 ms TTL=128
Réponse de 10.111.222.223 : octets=32 temps=3 ms TTL=128

Statistiques Ping pour 10.111.222.223:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 2ms, Maximum = 4ms, Moyenne = 3ms
```
🌞 Capture de ping

[c lo](./ping.pcapng)

🌞 Sur le PC serveur

```powershell
PS C:\Users\chris\Downloads\netcat-1.11> .\nc64.exe -l -p 9999
```

🌞 Sur le PC serveur

```powershell
  TCP    0.0.0.0:9999           0.0.0.0:0              LISTENING
 [nc64.exe]
```

🌞 Echangez-vous des messages

```powershell
PS C:\Users\chris\Downloads\netcat-1.11> .\nc64.exe -l -p 9999
cc
???
bonsoir sale
gentil
toi meme
ta du ping
oui
99999999ms
```
🌞 Utilisez une commande qui permet de voir la connexion en cours

```powershell
PS C:\WINDOWS\system32> netstat -an | findstr 9999
  TCP    10.111.222.202:9999    10.111.222.223:39962   ESTABLISHED
```

🌞 Faites une capture Wireshark complète d'un échange

[c lo](./netcat1.pcapng)

🌞 Inversez les rôles

.\nc64.exe 10.111.222.223 9999

[c lo](./netcat2.pcapng)

🌞 Utilisez Wireshark pour capturer du trafic HTTP

[c lo](./traficHTTP.pcapng)

🌞 Pour les 5 applications

Steam :

```powershell
PS C:\> netstat -a -b -n | Select-String steam -Context 0,1
>>

>  [steam.exe]
    TCP    0.0.0.0:31234          0.0.0.0:0              LISTENING
>  [steam.exe]
    TCP    10.33.78.234:34603     23.33.233.98:443       ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.78.234:34604     23.33.233.98:443       ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.78.234:34605     23.72.250.139:443      ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.78.234:34606     23.217.238.254:443     ESTABLISHED
>  [steamwebhelper.exe]
    TCP    10.33.78.234:34622     216.239.36.223:443     ESTABLISHED
>  [steam.exe]
    TCP    127.0.0.1:34577        0.0.0.0:0              LISTENING
>  [steam.exe]
    TCP    127.0.0.1:34577        127.0.0.1:34586        ESTABLISHED
>  [steam.exe]
    TCP    127.0.0.1:34579        0.0.0.0:0              LISTENING
>  [steam.exe]
    TCP    127.0.0.1:34579        127.0.0.1:34585        ESTABLISHED
>  [steam.exe]
    TCP    127.0.0.1:34585        127.0.0.1:34579        ESTABLISHED
>  [steamwebhelper.exe]
    TCP    127.0.0.1:34586        127.0.0.1:34577        ESTABLISHED
>  [steamwebhelper.exe]
    TCP    192.168.56.1:139       0.0.0.0:0              LISTENING
>  [steam.exe]
    UDP    0.0.0.0:31234          *:*
```
- 34603 → connecté à 23.33.233.98:443
- 34604 → connecté à 23.33.233.98:443
- 34605 → connecté à 23.72.250.139:443
- 34606 → connecté à 23.217.238.254:443
- 34622 → connecté à 216.239.36.223:443

[c lo](./Steam.pcap)

Discord :

```powershell
    TCP    10.33.78.234:2702      162.159.137.232:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.78.234:2703      20.140.48.70:443       ESTABLISHED
   [SearchHost.exe]
    TCP    10.33.78.234:2704      162.159.136.234:443    ESTABLISHED
>  [Discord.exe]
    TCP    10.33.78.234:2705      150.171.70.254:443     ESTABLISHED
   [SearchHost.exe]
    TCP    127.0.0.1:6463         0.0.0.0:0              LISTENING
>  [Discord.exe]
    TCP    127.0.0.1:27060        0.0.0.0:0              LISTENING
   [steam.exe]
    UDP    0.0.0.0:51857          162.159.134.233:443
>  [Discord.exe]
    UDP    0.0.0.0:53135          162.159.61.3:443
   [msedgewebview2.exe]
    UDP    0.0.0.0:56309          162.159.136.232:443
>  [Discord.exe]
    UDP    0.0.0.0:58107          162.159.134.233:443
>  [Discord.exe]
    UDP    0.0.0.0:62343          *:*
    Dnscache
    UDP    0.0.0.0:65453          162.159.137.232:443
>  [Discord.exe]
    UDP    10.33.78.234:137       *:*
   Impossible d'obtenir les informations de propriétaire
```

- 2702 → connecté à 162.159.137.232:443
- 2703 → connecté à 20.140.48.70:443
- 2704 → connecté à 162.159.136.234:443
- 2705 → connecté à 150.171.70.254:443
- 51857 → connecté à 162.159.134.233:443
- 53135 → connecté à 162.159.61.3:443
- 56309 → connecté à 162.159.136.232:443
- 58107 → connecté à 162.159.134.233:443
- 65453 → connecté à 162.159.137.232:443

[c lo](./discord.pcap)

