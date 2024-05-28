# En vrac

Lignes de commandes, portions de scripts pour préparation «éventuelle» d'un script HELPER à la manière de KIAUH (pour le moment quelques notes)

## Aides

#=== utiles pour flash de MCU Klipper (extrait de KIAUH)
```
function get_usb_id() {
  unset mcu_list
  sleep 1
  mcus=$(find /dev/serial/by-id/* 2>/dev/null)

  for mcu in ${mcus}; do
    mcu_list+=("${mcu}")
  done
}
function get_uart_id() {
  unset mcu_list
  sleep 1
  mcus=$(find /dev -maxdepth 1 -regextype posix-extended -regex "^\/dev\/tty(AMA0|S0)$" 2>/dev/null)

  for mcu in ${mcus}; do
    mcu_list+=("${mcu}")
  done
}
function get_dfu_id() {
  unset mcu_list
  sleep 1
  mcus=$(lsusb | grep "DFU" | cut -d " " -f 6 2>/dev/null)

  for mcu in ${mcus}; do
    mcu_list+=("${mcu}")
  done
}
```

## Empêcher la mise à jour de certains paquets :
- via l'utilitaire «**armbian-config**»

`sudo armbian-config`
- via une ligne de commandes :

```
# Hold DTB, kernel & firmware packages (armbian-config to release them => caution!)
sudo apt-mark hold base-files linux-dtb-current-rockchip64 linux-image-current-rockchip64 linux-headers-current-rockchip64 armbian-config armbian-firmware armbian-plymouth-theme armbian-zsh
```

## Ajouter l'utilisateur mks au groupe gpio pour lui permettre d'y accéder

**Le groupe gpio n'existe pas** mais en accès sudo on peut utiliser les outils ( gpioinfo, gpiodetect, gpiomon, gpioset, gpioget )

```
# Add user 'mks' to 'gpio' group for GPIO access
if ! sudo usermod -aG gpio mks &>/dev/null; then
  echo -e "${R}Failed to add user 'mks' to group 'gpio'${NC}"
fi
```

## Synchronisation automatique du système de fichiers

```
CRON_ENTRY="*/10 * * * * /bin/sync"
if ! (crontab -l 2>/dev/null | grep "/bin/sync"); then
    (crontab -l 2>/dev/null; echo "$CRON_ENTRY") | crontab -
    echo "Sync command added to the crontab to run every 10 minutes."
else
    echo "The sync command is already in the crontab."
fi
```

## Accès aux réglages réseaux (Wifi):

```
# Start Network Manager Text User Interface to activate Wifi if supported
# (not natively with Tenda Wifi key used by QidiTech on Series3 printers !)
sudo nmtui
```

## Libérer un peu de place :

```
sudo apt clean -y
sudo apt autoclean -y
sudo apt autoremove -y
```

## Modifier la manière de gérer les noms d'interfaces réseaux pour revenir à l'ancien système (ethX, wlanX) :

```
# Add extraargs to armbianEnv.txt if not exists
# ifnames takes form of ethX, wlanX
FILE_PATH="/boot/armbianEnv.txt"
LINE_TO_ADD="extraargs=net.ifnames=0"
if ! grep -q "$LINE_TO_ADD" "$FILE_PATH"; then
    echo "$LINE_TO_ADD" | sudo tee -a "$FILE_PATH" > /dev/null
    echo "Added '$LINE_TO_ADD' to $FILE_PATH."
else
    echo "The line '$LINE_TO_ADD' already exists in $FILE_PATH."
fi
```

Sans ce paramètre, l'interface **eth0** serait nommée **end1** (altname)

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

## Paquets à ajouter si absents :

PAQUETS_PYTHON="python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev python3-serial"
PAQUET_INDISPENSABLE="git"

```
sudo apt update 
sudo apt install ${PAQUET_INDISPENSABLE} ${PAQUETS_PYTHON} -y
```

## Automontage des clés USB :

```
#!/bin/bash

# Remove old mount directory if it exists 
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
    sudo mkdir -p "$mount_point"
fi

# Change the ownership of the mount point to user 'mks'
sudo chown mks:mks "$mount_point"

# Reload udev rules
sudo udevadm control --reload-rules && sudo udevadm trigger

echo "Udev rule for USB automount with sync option is configured."
```

## Ajout du support ADB

```
#!/bin/bash

sudo apt-get install -y android-sdk-platform-tools-common && sudo cp /lib/udev/rules.d/51-android.rules /etc/udev/rules.d/
echo "udev rule created. You may need to restart your system or reload udev rules."
# Reload udev rules
sudo udevadm control --reload-rules && sudo udevadm trigger

```
## Rechercher si une section du fichier printer.cfg existe et n'est pas commentée

```
printer_cfg="/home/mks/printer_data/config/printer.cfg"
grep -R "^\[probe\]" "${printer_cfg}"
…
Le résultat de cette commande peut être placé dans une variable pour effectuer un test
abl=$(grep -R "^\[probe\]" "${printer_cfg}")
if [[ "${abl}" == "[probe]" ]]; then
	    sed -i 's/printer\.probe\[\"x_offset\"\]/printer\.configfile\.settings\.probe\.x_offset/g' "${printer_cfg}"
        sed -i 's/printer\.probe\[\"y_offset\"\]/printer.configfile.settings.probe.y_offset/g' "${printer_cfg}"
fi
…
```

## …
