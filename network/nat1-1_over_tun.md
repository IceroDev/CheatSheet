## NAT 1-1 over Tun VPN
Ceci permet de lier une adresse IP publique à une machine sur le réseau local d'un serveur VPN. Le but étant de ne pas ouvrir les ports de son routeur tout en ayant un accès externe depuis une ip publique.

## Exemple

Mon serveur VPN est accessible depuis une adresse IP publique OVHCloud. Ma machine est un raspberry PI3B+ sur un réseau local chez moi.

## Utilisation

### Sur le serveur VPS possédant le network VPN local

Je crée un routing de l'IP que j'ai préalablement ajoutée sur mon serveur VPS (automatique sur Virtualizor)

```
[root@vpn ~]# iptables -t nat -A PREROUTING -d $IPPub -j DNAT --to-destination $IPLoc
[root@vpn ~]# iptables -t nat -A POSTROUTING -j MASQUERADE
```
Où la variable $IPPub est votre IP publique et $IPLoc votre IP locale VPN (de préférence statique bien sûr.


### Sur le serveur demandant une IP Publique

Je dois ajouter l'IP publique à l'interface tun0 crée par le VPN, elle s'appellera tun0:0
Sur Raspbian (utilisé pour la cheatsheet), les interfaces se gèrent depuis des fichiers sur /etc/network/interfaces.d/$fichier, $fichier étant un nom de fichier choisi par moi-même.

Le contenu de mon fichier est donc :
```
auto tun0:0
    iface tun0:0 inet static
        address $IPPub/32
        gateway $Gateway
```
Où $IPPub/32 est votre IP publique et /32 le subnet ajouté et $Gateway la gateway de votre serveur VPN.

#### Obtenir la gateway de mon VPN

```
root@raspberrypi:~# ip r
0.0.0.0/1 via 10.8.0.1 dev tun0 
default via 10.8.0.1 dev tun0 onlink 
default via 192.168.1.1 dev eth0 proto dhcp src 192.168.1.210 metric 202 
default via 192.168.1.1 dev wlan0 proto dhcp src 192.168.1.144 metric 303 
10.8.0.0/24 dev tun0 proto kernel scope link src 10.8.0.2 
128.0.0.0/1 via 10.8.0.1 dev tun0 
137.74.71.118 via 192.168.1.1 dev eth0 
192.168.1.0/24 dev eth0 proto dhcp scope link src 192.168.1.210 metric 202 
192.168.1.0/24 dev wlan0 proto dhcp scope link src 192.168.1.144 metric 303
```
On voit ici les gateway des différentes interfaces réseau de mon raspberry. Il est ici connecté à 3 réseaux (wifi, ethernet et vpn). L'ip qui nous interesse est celle du tun0 en première ligne, 10.8.0.1

## Auteurs

- [@IceroDev](https://www.github.com/IceroDev)

## Remerciements

- AZeRY12356

  
