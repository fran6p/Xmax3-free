# Améliorer l'OS

On peut se passer de tous ces ajouts mais ce serait dommage de s'en priver, certains sont purement cosmétiques d'autres apportent plus de fonctionnalités.

## Bloquer certaines mises à jour du système d'exploitation

Les OS prévus pour les Single Board Computer (SBC), ordinateurs monocartes dont le plus connu est le Raspberry Pi gèrent moyennement bien les montées de version du noyau (kernel), c'est particulièrement le cas des distributions Armbian.

Des outils existent permettant d'empêcher la mise à jour de paquets (`linux-dtb-*, linux-image-*, linux-headers-*, armbian-firmware*`, …).

Normalement, ces paquets après installation de l'image système sont bloqués. A chaque connexion ssh, c'est normalement indiqué

<p align="center">
<img src="/Images/ssh-accueil-apt-mark-hold.jpg">
</p>

On peut utiliser l'outil «armbian-config» pour geler / dégeler ces mises à jour :
```
sudo armbian-config
```

La première option de l'écran permet d'accéder aux paramètres «sensibles» du système:

<p align="center">
<img src="/Images/armbian-config-accueil.jpg">
</p>

En choisissant cette option, le fond d'écran change de couleur pour rappeler le caractère «dangereux» de certains choix. On peut geler / dégeler les mises à jour à «éviter» :

<p align="center">
<img src="/Images/armbian-config-system-freeze-kernel.jpg">
</p>

On peut évidemment aussi faire cette manipulation en ligne de commande. La liste des paquets gelés peut être récupérée lors d'un «apt upgrade» ou après un «apt update»  suivi d'un «apt list --upgrable»:
```
mks@mkspi:~$ sudo apt upgrade
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Calcul de la mise à jour... Fait
Les paquets suivants ont été conservés :
armbian-config armbian-firmware-full armbian-plymouth-theme armbian-zsh base-files linux-dtb-current-rockchip64
linux-headers-current-rockchip64 linux-image-current-rockchip64
0 mis à jour, 0 nouvellement installés, 0 à enlever et 8 non mis à jour.
```

Connaissant le nom des paquets, la commande à utiliser est «apt-mark hold nom-du-paquet» (unhold pour dégeler):
```
sudo apt-mark hold base-files linux-dtb-current-rockchip64 linux-image-current-rockchip64 linux-headers-current-rockchip64 armbian-config armbian-firmware armbian-plymouth-theme armbian-zsh
```

## Synchronisation du système de fichiers

Ces systèmes monocartes apprécient moyennement l'extinction brutale (coupure d'alimentation) avec le risque de corruption du support mémoire (les eMMC sont un peu plus tolérantes que les cartes SD). On peut demander au système d'écrire régulièrement via une tâche «cron. Soit on édite manuellement cette tâche :

`crontab -e`

en ajoutant pour une synchronisation toutes les cinq minutes :

`*/5 * * * * /bin/sync`

Soit en une seule ligne de commandes :
```
(crontab -l 2>/dev/null; echo "*/5 * * * * /bin/sync") | crontab -
```

## Modifier la manière de gérer les noms d'interfaces réseaux

Les systèmes récents utilisent «systemd» qui donnent des noms «aléatoires» aux interfaces réseaux, je préfère l'ancien système de nom (ethX, wlanX avec X={0,1,2, …}

On peut en ajoutant une ligne dans le fichier **/boot/armbianEnv.txt** revenir à cet ancien nommage :
```
echo "extraargs=net.ifnames=0" | sudo tee -a "/boot/armbianEnv.txt" > /dev/null
```

On peut évidemment ajouter cette ligne (extraargs=net.ifnames=0) manuellement en éditant le fichier :

`sudo nano /boot/armbianEnv.txt`

**Pour être pris en compte, il faudra redémarrer le système**.

Sans cette modification, l'interface Ethernet (eth0) serait nommée end1

```
    mks@mkspi:~$ ip addr
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 76:09:8f:72:f7:44 brd ff:ff:ff:ff:ff:ff
        altname end1
        inet 192.168.1.126/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
           valid_lft 4958sec preferred_lft 4958sec
        inet6 fe80::62b:66de:8f1:6305/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
```

## Ajout de quelques paquets

```
sudo apt update
sudo apt install python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev python3-serial
```

## Automontage des clés USB

Le système d'exploitation originel montait automatiquement les supports de masse (clés USB) dans le dossier de stockage des Gcodes dans un dossier sda1 créé au montage puis supprimé au démontage (retrait du support).

C'est une fonctionnalité bien pratique qui n'est pas implémentée sur la nouvelle image du système.

On peut ajouter cette fonctionnalité manuellement:

- créer des règles UDEV indiquant au système quoi faire quand un périphérique de masse est inséré
- créer le dossier servant de point de montage
- attribuer ce point de montage à l'utilisateur mks
- recharger les règles UDEV
- tester

C'est l'objet du script shell suivant (contenu à recopier dans un fichier nommé comme vous voulez ( automontage.sh, par exemple), droits d'exécution (chmod +x …)) :
```
#!/bin/bash

# Remove old rule if it exists 
sudo rm -f /etc/udev/rules.d/99-usb_automount.rules

# Define the path for the udev rule file
udev_rule_path="/etc/udev/rules.d/99-usb_automount.rules"

# Define the mount point
mount_point="/home/mks/printer_data/gcodes/USB"

# Obtain UID and GID for the mks user
uid=$(id -u mks)
gid=$(id -g mks)

# Udev rule to add
udev_rule_add="ACTION==\"add\", SUBSYSTEMS==\"usb\", SUBSYSTEM==\"block\", KERNEL==\"sd*1\", ENV{ID_FS_USAGE}==\"filesystem\", RUN{program}+=\"/usr/bin/systemd-mount --no-block --automount=yes --collect --options uid=$uid,gid=$gid,sync,nofail \$devnode $mount_point\""

# Udev rule to remove
udev_rule_remove="ACTION==\"remove\", SUBSYSTEMS==\"usb\", SUBSYSTEM==\"block\", KERNEL==\"sd*1\", RUN+=\"/bin/sh -c '/bin/umount $mount_point'\""

# Write the rules to the udev rule file
{
    echo "$udev_rule_add"
    echo "$udev_rule_remove"
} | sudo tee "$udev_rule_path"

# Create the mount point directory if it doesn't exist
if [ ! -d "$mount_point" ]; then
    #sudo mkdir -p "$mount_point"
    mkdir -p "$mount_point"
fi

# Change the ownership of the mount point to user 'mks'
sudo chown mks:mks "$mount_point"

# Reload udev rules
sudo udevadm control --reload-rules && sudo udevadm trigger

echo "Udev rule for USB automount with sync option is configured."
```

Lors de l'introduction d'une clé USB, désormais, le système la gérera automatiquement 😉

1) Pas de clé insérée:
```
    mks@mkspi:~$ lsblk
    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    mmcblk1      179:0    0  14,6G  0 disk
    ├─mmcblk1p1  179:1    0   256M  0 part /boot
    └─mmcblk1p2  179:2    0  14,1G  0 part /var/log.hdd
                                           /
    mmcblk1boot0 179:32   0     4M  1 disk
    mmcblk1boot1 179:64   0     4M  1 disk
    zram0        251:0    0 455,5M  0 disk [SWAP]
    zram1        251:1    0    50M  0 disk /var/log
    zram2        251:2    0     0B  0 disk
``` 

2) Clé insérée (le périphérique SDA est détecté mais pas encore monté)
``` 
    mks@mkspi:~$ lsblk
    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    sda            8:0    1  14,4G  0 disk
    └─sda1         8:1    1  14,4G  0 part
    mmcblk1      179:0    0  14,6G  0 disk
    ├─mmcblk1p1  179:1    0   256M  0 part /boot
    └─mmcblk1p2  179:2    0  14,1G  0 part /var/log.hdd
                                           /
    mmcblk1boot0 179:32   0     4M  1 disk
    mmcblk1boot1 179:64   0     4M  1 disk
    zram0        251:0    0 455,5M  0 disk [SWAP]
    zram1        251:1    0    50M  0 disk /var/log
    zram2        251:2    0     0B  0 disk
    Dérouler  
``` 
3) La lecture du contenu de la clé monte le dossier là où il faut :
``` 
    mks@mkspi:~$ ls -l printer_data/gcodes/USB
    total 4
    -rwxr-xr-x 1 mks mks    0 30 mai    2024  JeTeVois
    drwxr-xr-x 2 mks mks 4096 31 janv. 16:03 'System Volume Information'
    mks@mkspi:~$ lsblk
    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    sda            8:0    1  14,4G  0 disk
    └─sda1         8:1    1  14,4G  0 part /home/mks/printer_data/gcodes/USB
    mmcblk1      179:0    0  14,6G  0 disk
    ├─mmcblk1p1  179:1    0   256M  0 part /boot
    └─mmcblk1p2  179:2    0  14,1G  0 part /var/log.hdd
                                           /
    mmcblk1boot0 179:32   0     4M  1 disk
    mmcblk1boot1 179:64   0     4M  1 disk
    zram0        251:0    0 455,5M  0 disk [SWAP]
    zram1        251:1    0    50M  0 disk /var/log
    zram2        251:2    0     0B  0 disk
``` 

4) Après retrait de la clé, le périphérique est bien démonté, le point de montage lui n'a pas disparu mais est vide (il pourrait parfaitement contenir des fichiers qui n'apparaitraient pas en cas de montage d'une clé 😉 ) :
``` 
    mks@mkspi:~$ lsblk
    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    mmcblk1      179:0    0  14,6G  0 disk
    ├─mmcblk1p1  179:1    0   256M  0 part /boot
    └─mmcblk1p2  179:2    0  14,1G  0 part /var/log.hdd
                                           /
    mmcblk1boot0 179:32   0     4M  1 disk
    mmcblk1boot1 179:64   0     4M  1 disk
    zram0        251:0    0 455,5M  0 disk [SWAP]
    zram1        251:1    0    50M  0 disk /var/log
    zram2        251:2    0     0B  0 disk
    mks@mkspi:~$ ls -l printer_data/gcodes/USB
    total 0
``` 

## Accéder aux dossiers de l'utilisateur MKS via l'explorateur de fichiers de Windows

Même si on peut accéder via l'interface Web (Fluidd / Mainsail) à certains dossiers de Klipper avec un navigateur, l'explorateur de fichiers permet en plus (si l'utilisateur mks a bien été ajouté 😉 ) l'accès total au répertoire personnel de «mks»

Pour réaliser «cette magie», rien de bien sorcier:

- Installer quelques paquets:
``` 
    sudo apt install && sudo apt install samba winbind
``` 
- Éditer le fichier /etc/samba/smb.conf:
``` 
    sudo nano /etc/samba/smb.conf
``` 
- Ajouter à la fin de ce fichier :

``` 
        [Print_Files]
        comment = GCode_files
        path = /home/mks/printer_data/gcodes
        browseable = Yes
        writeable = Yes
        only guest = no
        create mask = 0770
        directory mask = 0770
        public = yes
        read only = no
        force user = mks
        force group = mks
         
        [Klipper_Configs]
        comment = Klipper configurations
        path = /home/mks/printer_data/config
        browseable = Yes
        writeable = Yes
        only guest = no
        create mask = 0770
        directory mask = 0770
        public = yes
        read only = no
        force user = mks
        force group = mks

        [Timelapses]
        comment = Timelapses
        path = /home/mks/timelapse
        browseable = Yes
        writeable = Yes
        only guest = no
        create mask = 0770
        directory mask = 0770
        public = yes
        read only = no
        force user = mks
        force group = mks
``` 

- Enregistrer le fichier modifié
- Ajouter l'utilisateur mks pour lui permettre l'accès à son «home» depuis Windows
``` 
    sudo smbpasswd -a mks
``` 
- Redémarrer le daemon système :
``` 
    sudo systemctl restart smbd
``` 

Accès au partage \\adr.ess.e.ip

<p align="center">
<img src="/Images/samba-xmax3.jpg">
</p>

Accès au dossier «Home»

<p align="center">
<img src="/Images/samba-mks.jpg">
</p>

## Agrandir la partition système

L'installation du nouveau système a d'abord été faite sur l'ancienne eMMC de 8 Go. Régulièrement, des images du contenu ont été sauvegardées à l'aide de mon utilitaire favori [imageUSB](https://www.osforensics.com/tools/write-usb-images.html).

Au départ, la place occupée représentait à peu près moins de la moitié de la capacité totale.
``` 
mks@mkspi:~$ df -h
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               379M       0  379M   0% /dev
tmpfs               92M    2,4M   89M   3% /run
/dev/mmcblk1p2     6,7G    3,6G  3,1G  55% /
tmpfs              456M       0  456M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
tmpfs              456M     16K  456M   1% /tmp
/dev/mmcblk1p1     256M     88M  169M  35% /boot
/dev/zram1          47M    960K   43M   3% /var/log
tmpfs               92M       0   92M   0% /run/user/1000
``` 
Au fil des installations liées à l'écosystème Klipper (Klipper, Moonraker, Fluidd, Mainsail, Crowsnest, Mobileraker, PrettyGCode, OctoEverywhere, Spoolman, …) je me retrouvais comme avec l'image du système de Qidi avec moins de 1 Go disponible (système utilisé à plus de 90%)
``` 
mks@mkspi:~$ df -H
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               398M       0  398M   0% /dev
tmpfs               96M    4,3M   92M   5% /run
/dev/mmcblk1p2     7,2G    6,4G  722M  90% /
tmpfs              478M       0  478M   0% /dev/shm
tmpfs              5,3M       0  5,3M   0% /run/lock
tmpfs              478M    8,2k  478M   1% /tmp
/dev/mmcblk1p1     269M     92M  177M  35% /boot
/dev/zram1          50M    3,8M   42M   9% /var/log
tmpfs               96M       0   96M   0% /run/user/1000
``` 
J'ai donc ré-imagé un système sur une eMMC de 16 Go à partir d'une image enregistrée de 8 Go. Pour prendre en compte la capacité de cette nouvelle mémoire, il faut procéder à un agrandissement de la partition (nul besoin de pompe comme pour d'autres agrandissements 😄 ).
``` 
sudo systemctl enable armbian-resize-filesystem && sudo reboot
``` 
Après redémarrage, la partition a été agrandie
``` 
mks@mkspi:~$ df -h
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               379M       0  379M   0% /dev
tmpfs               92M    4,2M   87M   5% /run
/dev/mmcblk1p2      14G    6,1G  7,6G  45% /
tmpfs              456M       0  456M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
tmpfs              456M    4,0K  456M   1% /tmp
/dev/mmcblk1p1     256M     88M  169M  35% /boot
/dev/zram1          47M    2,7M   41M   7% /var/log
tmpfs               92M       0   92M   0% /run/user/1000
```

## Wifi

La clé Wifi originelle n'est pas reconnue. En fait si, elle l'est mais le firmware nécessaire à la gestion de la puce n'a pas été inclus dans l'image Armbian.

Avec `lsusb` elle apparait correctement dans les périphériques USB:
``` 
    mks@mkspi:~$ lsusb
    Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Bus 002 Device 004: ID 0bda:b711 Realtek Semiconductor Corp. RTL8188GU 802.11n WLAN Adapter (After Modeswitch)
    Bus 002 Device 002: ID 214b:7250 Huasheng Electronics USB2.0 HUB
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

    …
``` 

Ça permet a minima de connaitre le chipset (puce) utilisé par cette clé (RTL8188GU)

Cette clé est d'abord détectée comme un stockage de masse, l'utilitaire «Modeswitch» la reconnait et la bascule en adaptateur Wifi.

Malheureusement, le firmware lié au chipset RTL8188GU ne peut pas être chargé, ce que confirme `dmesg` :

``` 
    [  838.999696] usb 2-1.2: new high-speed USB device number 3 using xhci-hcd
    [  839.100243] usb 2-1.2: New USB device found, idVendor=0bda, idProduct=1a2b, bcdDevice= 2.00
    [  839.100278] usb 2-1.2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
    [  839.100296] usb 2-1.2: Product: DISK
    [  839.100310] usb 2-1.2: Manufacturer: Realtek
    [  839.102183] usb-storage 2-1.2:1.0: USB Mass Storage device detected
    [  839.103478] scsi host0: usb-storage 2-1.2:1.0
    [  839.435114] usbcore: registered new interface driver uas
    [  840.196370] usb 2-1.2: USB disconnect, device number 3
    [  840.419681] usb 2-1.2: new high-speed USB device number 4 using xhci-hcd
    [  840.520036] usb 2-1.2: New USB device found, idVendor=0bda, idProduct=b711, bcdDevice= 2.00
    [  840.520058] usb 2-1.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [  840.520066] usb 2-1.2: Product: 802.11n WLAN Adapter
    [  840.520073] usb 2-1.2: Manufacturer: Realtek
    [  840.520079] usb 2-1.2: SerialNumber: 00E04CB82101
    [  840.626327] cfg80211: Loading compiled-in X.509 certificates for regulatory database
    [  840.627742] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
    [  840.628595] Loaded X.509 cert 'wens: 61c038651aabdcf94bd0ac7ff06c7248db18c600'
    [  840.634514] cfg80211: loaded regulatory.db is malformed or signature is missing/invalid
    [  840.688876] usb 2-1.2: RTL8710BU rev A (SMIC) romver 1, 1T1R, TX queues 3, WiFi=1, BT=0, GPS=0, HI PA=0
    [  840.688901] usb 2-1.2: RTL8710BU MAC: 50:2b:73:a0:6e:d4
    [  840.688910] usb 2-1.2: rtl8xxxu: Loading firmware rtlwifi/rtl8710bufw_SMIC.bin
    [  840.689468] usb 2-1.2: Direct firmware load for rtlwifi/rtl8710bufw_SMIC.bin failed with error -2
    [  840.689492] usb 2-1.2: request_firmware(rtlwifi/rtl8710bufw_SMIC.bin) failed
    [  840.689501] usb 2-1.2: Fatal - failed to load firmware
    [  840.689535] rtl8xxxu: probe of 2-1.2:1.0 failed with error -11
    [  840.689684] usbcore: registered new interface driver rtl8xxxu
``` 

Le module lié à ce chipset est pourtant bien chargé :
``` 
mks@mkspi:~$ sudo lsmod | grep rtl
rtl8xxxu              204800  0
mac80211             1011712  1 rtl8xxxu
cfg80211              991232  2 mac80211,rtl8xxxu
``` 
Aucune interface réseau Wifi n'est disponible :
``` 
mks@mkspi:~$ sudo iwconfig
lo        no wireless extensions.

eth0      no wireless extensions.
``` 
J'ai d'autres clés Wifi fonctionnelles sous Linux. Après test, elles fonctionnent avec cette distribution Armbian. [Celle-ci](https://www.amazon.fr/dp/B098J4WHYY) (plus efficace) avec son chipset RTW8821C est parfaitement reconnue (y compris le Bluetooth après ajout des paquets «kivonbi1»).

C'est donc un problème avec la clé Tenda et sa puce RTL8188GU

N'ayant pas trop envie (ni besoin car j'utilise des connexions filaires) de compiler module et firmware, je tente après dégel du paquet «armbian-firmware», d'installer le firmware complet (armbian-firmware-full). Je dois également dégeler les autres paquets «armbian-*» et le «basefiles»

Le système veut bien mettre à jour…

Juste un GROS moment d'angoisse quand à la fin de cette installation, le système m'annonce qu'il recrée une «initram» (la dernière fois que j'avais eu ce message avec l'image originelle, au reboot suivant plus rien ne fonctionnait). De toute façon, la mise à jour s'est faite donc on se…re les fesses, sort les amulettes, prie St Murphy, etc. et lance un redémarrage (reboot)

OUF 🥳 Le système redémarre sans broncher… La clé, cette fois-ci est détectée et fonctionnelle :

``` 
    mks@mkspi:~$ lsusb
    Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Bus 002 Device 004: ID 0bda:b711 Realtek Semiconductor Corp. RTL8188GU 802.11n WLAN Adapter (After Modeswitch)
    Bus 002 Device 002: ID 214b:7250 Huasheng Electronics USB2.0 HUB
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 004 Device 003: ID 1bcf:0b09 Sunplus Innovation Technology Inc. HD Camera
    Bus 004 Device 002: ID 1a40:0101 Terminus Technology Inc. Hub
    Bus 004 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 002: ID 1d50:614e OpenMoko, Inc. rp2040
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    mks@mkspi:~$ sudo lsmod|grep rtl
    rtl8xxxu              204800  0
    mac80211             1011712  1 rtl8xxxu
    cfg80211              991232  2 mac80211,rtl8xxxu

    mks@mkspi:~$ sudo iwconfig
    lo        no wireless extensions.

    eth0      no wireless extensions.

    wlan0     IEEE 802.11  ESSID:"WESTEROS-NOVA"
              Mode:Managed  Frequency:2.437 GHz  Access Point: E8:65:D4:58:DC:41
              Bit Rate=72.2 Mb/s   Tx-Power=20 dBm
              Retry short limit:7   RTS thr=2347 B   Fragment thr:off
              Encryption key:off
              Power Management:off
              Link Quality=70/70  Signal level=-36 dBm
              Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
              Tx excessive retries:0  Invalid misc:4   Missed beacon:0

    mks@mkspi:~$ ip addr
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 76:09:8f:72:f7:44 brd ff:ff:ff:ff:ff:ff
        altname end1
        inet 192.168.1.126/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
           valid_lft 5580sec preferred_lft 5580sec
        inet6 fe80::62b:66de:8f1:6305/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
    3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
        link/ether 50:2b:73:a0:6e:d4 brd ff:ff:ff:ff:ff:ff
        inet 192.168.1.125/24 brd 192.168.1.255 scope global dynamic noprefixroute wlan0
           valid_lft 5582sec preferred_lft 5582sec
        inet6 fe80::3d0c:b46b:56ab:edb7/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
``` 

dmesg:
```     
    [   18.953660] usb 2-1.2: USB disconnect, device number 3
    [   19.186657] usb 2-1.2: new high-speed USB device number 4 using xhci-hcd
    [   19.291684] usb 2-1.2: New USB device found, idVendor=0bda, idProduct=b711, bcdDevice= 2.00
    [   19.291711] usb 2-1.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [   19.291720] usb 2-1.2: Product: 802.11n WLAN Adapter
    [   19.291726] usb 2-1.2: Manufacturer: Realtek
    [   19.291733] usb 2-1.2: SerialNumber: 00E04CB82101
    [   19.435300] cfg80211: Loading compiled-in X.509 certificates for regulatory database
    [   19.436954] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
    [   19.437802] Loaded X.509 cert 'wens: 61c038651aabdcf94bd0ac7ff06c7248db18c600'
    [   19.451371] cfg80211: loaded regulatory.db is malformed or signature is missing/invalid
    [   19.635598] usb 2-1.2: RTL8710BU rev A (SMIC) romver 1, 1T1R, TX queues 3, WiFi=1, BT=0, GPS=0, HI PA=0
    [   19.635627] usb 2-1.2: RTL8710BU MAC: 50:2b:73:a0:6e:d4
    [   19.635637] usb 2-1.2: rtl8xxxu: Loading firmware rtlwifi/rtl8710bufw_SMIC.bin
    [   19.643774] usb 2-1.2: Firmware revision 16.0 (signature 0x10b1)
    [   19.915630] rk_gmac-dwmac ff550000.ethernet eth0: Register MEM_TYPE_PAGE_POOL RxQ-0
    [   19.980948] rk_gmac-dwmac ff550000.ethernet eth0: PHY [stmmac-1:00] driver [Rockchip integrated EPHY] (irq=POLL)
    [   19.990684] rk_gmac-dwmac ff550000.ethernet eth0: No Safety Features support found
    [   19.990721] rk_gmac-dwmac ff550000.ethernet eth0: PTP not supported by HW
    [   19.992052] rk_gmac-dwmac ff550000.ethernet eth0: configuring for phy/rmii link mode
    [   20.786933] usbcore: registered new interface driver rtl8xxxu
    [   22.053583] rk_gmac-dwmac ff550000.ethernet eth0: Link is Up - 100Mbps/Full - flow control off
    [   22.544823] wlan0: authenticate with e8:65:d4:58:dc:41
    [   22.553388] wlan0: send auth to e8:65:d4:58:dc:41 (try 1/3)
    [   22.562527] wlan0: authenticated
    [   22.567301] wlan0: associate with e8:65:d4:58:dc:41 (try 1/3)
    [   22.585438] wlan0: RX AssocResp from e8:65:d4:58:dc:41 (capab=0x411 status=0 aid=11)
    [   22.587735] usb 2-1.2: rtl8xxxu_bss_info_changed: HT supported
    [   22.592575] wlan0: associated

``` 

Je n'ai même pas eu besoin de saisir les informations (SSID, mot de passe du point d'accès), les infos de connexions, utilisées lors du test fait avec mon autre clé Wifi ont été réutilisées.

Sinon, pour ajouter et paramétrer le réseau Wifi:
``` 
sudo nmtui
``` 
 
On peut également utiliser l'utilitaire en ligne de commandes nmcli
