## SFTP Disk
Ceci nous permet de créer un disque dur virtuel sur une machine de façon à utiliser le stockage d'une autre. Ceci est très pratique pour garder par exemple ses données chez soi sans directement passer par le serveur de stockage.

## Exemple
J'utilise un VPS OVHCloud limité dû à leur prix. Ce VPS me permet d'avoir des adresses IP personnalisées à mon nom. Le problème est que le stockage est extrèmement limité (10Go).
La solution est de simplement utiliser ce système pour me connecter à mon RaspberryPI qui servira à jouer le rôle de disque dur chez moi. Cela me permet de garder mes données sur un stockage étendu qui plus est est chez moi, me donnant un accès direct aux données.

## Remarque
J'ai essayé ce système avec comme récepteur un VPS sous virtualisation LXC. Il semblerait qu'un container LXC n'ait pas le droit de modifier ses accès à des disques.

## Utilisation
### Installations
```
$ sudo apt update
$ sudo apt install sshfs
```
### Préparation du montage
Votre SFTP va devoir être monté sur un endroit de votre VPS. Vous pouvez le lier à un dossier existant ou en recréer un.

#### En règle générale
```
$ sshfs [user@]host:[dossier] mountpoint [paramètres]
```

#### Exemple
Dans cet exemple, le serveur se connecte à l'utilisateur "jean" sur l'ip 192.168.1.17 et va "stream" le dossier /var/www/html sur le dossier /root/mount/rpi de la machine où vous effectuez la commande.
```
$ sshfs jean@192.168.1.17:/var/www/html /root/mount/rpi
```
#### Utilisation d'un port différent de 22
Il est possible que vous n'utilisiez pas le port 22 pour votre session SSH, dans ce cas, il vous suffit d'ajouter le paramètre "p"
```
$ sshfs jean@192.168.1.17:/var/www/html /root/mount/rpi -p 2678
```

Entrez ensuite simplement votre mot de passe SSH pour autoriser la connexion au serveur

## Vérifications
Vérifiez la connexion et le point de montade de votre serveur en faisant simplement la commande "df"

```
root@s1-2-gra11 ~ # df -h
Filesystem                            Size  Used Avail Use% Mounted on
udev                                  967M     0  967M   0% /dev
tmpfs                                 195M   23M  173M  12% /run
/dev/sda1                             9.9G  1.4G  8.0G  15% /
tmpfs                                 975M     0  975M   0% /dev/shm
tmpfs                                 5.0M     0  5.0M   0% /run/lock
tmpfs                                 975M     0  975M   0% /sys/fs/cgroup
tmpfs                                 195M     0  195M   0% /run/user/0
jean@192.168.1.17:/var/www/html       1.6T   38G  1.5T   3%  /root/mount/rpi
```
Votre connexion est bien présente, il s'agit de la dernière ligne.

## Auteurs

- [@IceroDev](https://www.github.com/IceroDev)

  
