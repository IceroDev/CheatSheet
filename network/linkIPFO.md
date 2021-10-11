## Link IPFO
Ceci permet de lier des adresses IP Failover à un service VPS ou instance OVHCloud. Ces manipulations ne sont pas demandées sur Virtualizor où le système crée ces fichiers automatiquement lors de l'initialisation de la machine. 

## Exemple
Mon serveur VPS est accessible depuis une adresse IP publique OVHCloud. Mais j'utilise des adresses IP failover pour héberger mes sites internet sur des adresses IP différentes étant des failover.

## Utilisation
Le fichier a modifier est généralement situé sous le path /etc/network/interfaces.d/50-cloud-init
Votre fichier devrait ressembler à cela :

```
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
auto lo
iface lo inet loopback
    dns-nameservers XX.XX.XX.XX

auto eth0
iface eth0 inet dhcp
    mtu 1500

```
Où XX.XX.XX.XX est votre adresse DNS fournie par votre provider.

Pour ajouter vos failover, ajoutez simplement selon la syntaxe suivante :
```
auto eth0:[n°]
iface eth0:[n°] inet static
    address YY.YY.YY.YY
```
où [n°] est simplement un compteur à partir de 0
où YY.YY.YY.YY est votre adresse IPFO

Un fichier complet serait par exemple :

```
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
auto lo
iface lo inet loopback
    dns-nameservers XX.XX.XX.XX

auto eth0
iface eth0 inet dhcp
    mtu 1500

auto eth0:0
iface eth0:0 inet static
    address YY.YY.YY.YY

auto eth0:1
iface eth0:1 inet static
    address ZZ.ZZ.ZZ.ZZ

auto eth0:2
iface eth0:2 inet static
    address WW.WW.WW.WW

auto eth0:3
iface eth0:3 inet static
    address VV.VV.VV.VV
```
où YY.YY.YY.YY/ZZ.ZZ.ZZ.ZZ/WW.WW.WW.WW/VV.VV.VV.VV sont vos adresses IPFO

Une fois les modifications faites, redémarrez le networking

```
$ systemctl restart networking
``` 

## Auteurs

- [@IceroDev](https://www.github.com/IceroDev)

## Remerciements

- [@URKAhub](https://github.com/URKAhub)

  
