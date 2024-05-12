En vrac

Empêcher la mise à jour de certains paquets :
- via l'utilitaire «armban-config»

`sudo armbian-config`
- via une ligne de commandes :

```
# Hold DTB, kernel & firmware packages (armbian-config to release them => caution!)
sudo apt-mark hold base-files linux-dtb-current-rockchip64 linux-image-current-rockchip64 linux-headers-current-rockchip64 armbian-config armbian-firmware armbian-plymouth-theme armbian-zsh
```

Synchronisation auto du système de fichiers

```
CRON_ENTRY="*/10 * * * * /bin/sync"
if ! (crontab -l 2>/dev/null | grep "/bin/sync"); then
    (crontab -l 2>/dev/null; echo "$CRON_ENTRY") | crontab -
    echo "Sync command added to the crontab to run every 10 minutes."
else
    echo "The sync command is already in the crontab."
fi
```

Accès au réglages réseaux :

```
sudo nmtui
```

Libérer un peu de place :

```
sudo apt clean -y
sudo apt autoclean -y
sudo apt autoremove -y
```

Modifier la manière de gérer les noms d'interfaces réseaux pour revenir à l'ancien système (ethX, wlanX) :

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

Paquets à ajouter si absents :
PAQUETS_PYTHON="python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev python3-serial"
PAQUET_INDISPENSABLE="git"
PAQUET_CAMERA="ustreamer"

```
udo apt update 
sudo apt install ${PAQUET_INDISPENSABLE} ${PAQUETS_PYTHON} ${PAQUET_CAMERA} -y
```

Automontage des clés USB :

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

Ajout du support ADB

```
#!/bin/bash

sudo apt-get install -y android-sdk-platform-tools-common && sudo cp /lib/udev/rules.d/51-android.rules /etc/udev/rules.d/
echo "udev rule created. You may need to restart your system or reload udev rules."
# Reload udev rules
sudo udevadm control --reload-rules && sudo udevadm trigger

```

…
