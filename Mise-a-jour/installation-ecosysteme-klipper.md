# KIAUH (Klipper Installation And Update Helper)

Un ensemble de scripts permet une installation facilitée des éléments composants l'**«écosystème Klipper»**. Grâce à celui-ci, il est possible d'installer, de mettre à jour, de supprimer des éléments de la suite, juste en choisissant les *bonnes* options.

On commence par récupérer le dépôt Github de cet outil puis on exécutera son script shell.

1. installer le paquet "git" (au cas où il serait absent (*ce qui ne devrait pas être le cas avec l'installation de la distribution Armbian Bookworm* 🤔))

```
sudo apt update && sudo apt install git -y
```
2. cloner le dépôt

```
cd && git clone https://github.com/dw-0/kiauh.git
```

<details>
 
<summary>Installation de KIAUH</summary>
 
```
mks@mkspi:~$ cd && git clone https://github.com/dw-0/kiauh.git
Clonage dans 'kiauh'...
remote: Enumerating objects: 5584, done.
remote: Counting objects: 100% (5584/5584), done.
remote: Compressing objects: 100% (1673/1673), done.
remote: Total 5584 (delta 4043), reused 5274 (delta 3879), pack-reused 0
Réception d'objets: 100% (5584/5584), 1.46 Mio | 4.99 Mio/s, fait.
Résolution des deltas: 100% (4043/4043), fait.
```
  
</details>

3. Exécuter le script

```
~/kiauh/kiauh.sh
```

La première fenêtre donne accès au menu principal de KIAUH
![](../Images/kiauh-installation-vierge.jpg)

Rien n'est encore installé. Choisir l'option (1) pour accéder au menu des installations
![](../Images/kiauh-installation-choix.jpg)

**Installer dans l'ordre** :
1. Klipper
2. Moonraker
3. Mainsail
4. Fluidd (port 10088)

### KLIPPER

Aprés sélection (1), choisir la version de Python (v3 recommandée)
![](../Images/kiauh-installation-klipper-python3.jpg)

Nombre d'instances de Klipper (plusieurs imprimantes)
![](../Images/kiauh-installation-klipper-python3-une-instance.jpg)

<details>

<summary>Installation de Klipper</summary>
    
```
###### Installing single Klipper instance ...

###### Checking for the following dependencies:
● git
[✓ OK] Dependencies already met!

###### Cloning Klipper from https://github.com/Klipper3d/klipper ...
Clonage dans '/home/mks/klipper'...
remote: Enumerating objects: 60918, done.
remote: Counting objects: 100% (311/311), done.
remote: Compressing objects: 100% (164/164), done.
remote: Total 60918 (delta 237), reused 178 (delta 139), pack-reused 60607
Réception d'objets: 100% (60918/60918), 171.75 Mio | 5.25 Mio/s, fait.
Résolution des deltas: 100% (48823/48823), fait.
Mise à jour des fichiers: 100% (2129/2129), fait.
Déjà sur 'master'
Votre branche est à jour avec 'origin/master'.

###### Reading dependencies...
virtualenv
python3-dev
libffi-dev
build-essential
libncurses-dev
libusb-dev
avrdude
gcc-avr
binutils-avr
avr-libc
stm32flash
libnewlib-arm-none-eabi
gcc-arm-none-eabi
binutils-arm-none-eabi
libusb-1.0
pkg-config
dfu-util

###### Installing packages...
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Note : sélection de libusb-1.0-0-dev pour l'expression rationnelle « libusb-1.0 »
Note : sélection de libusb-1.0-doc pour l'expression rationnelle « libusb-1.0 »
Note : sélection de libusb-1.0-0 pour l'expression rationnelle « libusb-1.0 »
build-essential est déjà la version la plus récente (12.9).
libusb-1.0-0 est déjà la version la plus récente (2:1.0.26-1).
libusb-1.0-0 passé en « installé manuellement ».
Les paquets supplémentaires suivants seront installés :
  libexpat1-dev libftdi1 libhidapi-libusb0 libjs-jquery libjs-sphinxdoc
  libjs-underscore libncurses6 libnewlib-dev libpkgconf3 libpython3-dev
  libpython3.11 libpython3.11-dev libusb-0.1-4 pkgconf pkgconf-bin
  python3-distlib python3-filelock python3-pip-whl python3-platformdirs
  python3-setuptools-whl python3-virtualenv python3-wheel-whl python3.11-dev
Paquets suggérés :
  dfu-programmer avrdude-doc gcc-doc ncurses-doc libnewlib-doc
Paquets recommandés :
  javascript-common libstdc++-arm-none-eabi-newlib
Les NOUVEAUX paquets suivants seront installés :
  avr-libc avrdude binutils-arm-none-eabi binutils-avr dfu-util
  gcc-arm-none-eabi gcc-avr libexpat1-dev libffi-dev libftdi1
  libhidapi-libusb0 libjs-jquery libjs-sphinxdoc libjs-underscore
  libncurses-dev libncurses6 libnewlib-arm-none-eabi libnewlib-dev libpkgconf3
  libpython3-dev libpython3.11 libpython3.11-dev libusb-0.1-4 libusb-1.0-0-dev
  libusb-1.0-doc libusb-dev pkg-config pkgconf pkgconf-bin python3-dev
  python3-distlib python3-filelock python3-pip-whl python3-platformdirs
  python3-setuptools-whl python3-virtualenv python3-wheel-whl python3.11-dev
  stm32flash virtualenv
0 mis à jour, 40 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 124 Mo dans les archives.
Après cette opération, 1035 Mo d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian bookworm/main arm64 binutils-avr arm64 2.26.20160125+Atmel3.6.2-4 [1395 kB]
Réception de :2 http://deb.debian.org/debian bookworm/main arm64 gcc-avr arm64 1:5.4.0+Atmel3.6.2-3 [14,1 MB]
Réception de :3 http://deb.debian.org/debian bookworm/main arm64 avr-libc all 1:2.0.0+Atmel3.6.2-3 [4889 kB]
Réception de :4 http://deb.debian.org/debian bookworm/main arm64 libusb-0.1-4 arm64 2:0.1.12-32 [21,5 kB]
Réception de :5 http://deb.debian.org/debian bookworm/main arm64 libftdi1 arm64 0.20-4+b1 [18,8 kB]
Réception de :6 http://deb.debian.org/debian bookworm/main arm64 libhidapi-libusb0 arm64 0.13.1-1 [15,3 kB]
Réception de :7 http://deb.debian.org/debian bookworm/main arm64 avrdude arm64 7.1+dfsg-3 [366 kB]
Réception de :8 http://deb.debian.org/debian bookworm/main arm64 binutils-arm-none-eabi arm64 2.40-2+18+b1 [2694 kB]
Réception de :9 http://deb.debian.org/debian bookworm/main arm64 dfu-util arm64 0.11-1 [41,2 kB]
Réception de :10 http://deb.debian.org/debian bookworm/main arm64 gcc-arm-none-eabi arm64 15:12.2.rel1-1 [45,1 MB]
Réception de :11 http://deb.debian.org/debian bookworm/main arm64 libexpat1-dev arm64 2.5.0-1 [135 kB]
Réception de :12 http://deb.debian.org/debian bookworm/main arm64 libffi-dev arm64 3.4.4-1 [56,0 kB]
Réception de :13 http://deb.debian.org/debian bookworm/main arm64 libjs-jquery all 3.6.1+dfsg+~3.5.14-1 [326 kB]
Réception de :14 http://deb.debian.org/debian bookworm/main arm64 libjs-underscore all 1.13.4~dfsg+~1.11.4-3 [116 kB]
Réception de :15 http://deb.debian.org/debian bookworm/main arm64 libjs-sphinxdoc all 5.3.0-4 [130 kB]
Réception de :16 http://deb.debian.org/debian bookworm/main arm64 libncurses6 arm64 6.4-4 [93,9 kB]
Réception de :17 http://deb.debian.org/debian bookworm/main arm64 libncurses-dev arm64 6.4-4 [336 kB]
Réception de :18 http://deb.debian.org/debian bookworm/main arm64 libnewlib-dev all 3.3.0-1.3 [262 kB]
Réception de :19 http://deb.debian.org/debian bookworm/main arm64 libnewlib-arm-none-eabi all 3.3.0-1.3 [43,6 MB]
Réception de :20 http://deb.debian.org/debian bookworm/main arm64 libpkgconf3 arm64 1.8.1-1 [35,3 kB]
Réception de :21 http://deb.debian.org/debian bookworm/main arm64 libpython3.11 arm64 3.11.2-6 [1841 kB]
Réception de :22 http://deb.debian.org/debian bookworm/main arm64 libpython3.11-dev arm64 3.11.2-6 [4395 kB]
Réception de :23 http://deb.debian.org/debian bookworm/main arm64 libpython3-dev arm64 3.11.2-1+b1 [9564 B]
Réception de :24 http://deb.debian.org/debian bookworm/main arm64 libusb-1.0-0-dev arm64 2:1.0.26-1 [83,0 kB]
Réception de :25 http://deb.debian.org/debian bookworm/main arm64 libusb-1.0-doc all 2:1.0.26-1 [193 kB]
Réception de :26 http://deb.debian.org/debian bookworm/main arm64 libusb-dev arm64 2:0.1.12-32 [37,5 kB]
Réception de :27 http://deb.debian.org/debian bookworm/main arm64 pkgconf-bin arm64 1.8.1-1 [28,9 kB]
Réception de :28 http://deb.debian.org/debian bookworm/main arm64 pkgconf arm64 1.8.1-1 [25,9 kB]
Réception de :29 http://deb.debian.org/debian bookworm/main arm64 pkg-config arm64 1.8.1-1 [13,7 kB]
Réception de :30 http://deb.debian.org/debian bookworm/main arm64 python3.11-dev arm64 3.11.2-6 [617 kB]
Réception de :31 http://deb.debian.org/debian bookworm/main arm64 python3-dev arm64 3.11.2-1+b1 [26,2 kB]
Réception de :32 http://deb.debian.org/debian bookworm/main arm64 python3-distlib all 0.3.6-1 [257 kB]
Réception de :33 http://deb.debian.org/debian bookworm/main arm64 python3-filelock all 3.9.0-1 [9460 B]
Réception de :34 http://deb.debian.org/debian bookworm/main arm64 python3-pip-whl all 23.0.1+dfsg-1 [1717 kB]
Réception de :35 http://deb.debian.org/debian bookworm/main arm64 python3-platformdirs all 2.6.0-1 [16,3 kB]
Réception de :36 http://deb.debian.org/debian bookworm/main arm64 python3-setuptools-whl all 66.1.1-1 [1111 kB]
Réception de :37 http://deb.debian.org/debian bookworm/main arm64 python3-wheel-whl all 0.38.4-2 [38,6 kB]
Réception de :38 http://deb.debian.org/debian bookworm/main arm64 python3-virtualenv all 20.17.1+ds-1 [93,9 kB]
Réception de :39 http://deb.debian.org/debian bookworm/main arm64 stm32flash arm64 0.7-1 [31,4 kB]
Réception de :40 http://deb.debian.org/debian bookworm/main arm64 virtualenv all 20.17.1+ds-1 [21,1 kB]
124 Mo réceptionnés en 13s (9570 ko/s)
Extracting templates from packages: 100%
Sélection du paquet binutils-avr précédemment désélectionné.
(Lecture de la base de données... 68159 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../00-binutils-avr_2.26.20160125+Atmel3.6.2-4_arm64.deb ...
Dépaquetage de binutils-avr (2.26.20160125+Atmel3.6.2-4) ...
Sélection du paquet gcc-avr précédemment désélectionné.
Préparation du dépaquetage de .../01-gcc-avr_1%3a5.4.0+Atmel3.6.2-3_arm64.deb ...
Dépaquetage de gcc-avr (1:5.4.0+Atmel3.6.2-3) ...
Sélection du paquet avr-libc précédemment désélectionné.
Préparation du dépaquetage de .../02-avr-libc_1%3a2.0.0+Atmel3.6.2-3_all.deb ...
Dépaquetage de avr-libc (1:2.0.0+Atmel3.6.2-3) ...
Sélection du paquet libusb-0.1-4:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../03-libusb-0.1-4_2%3a0.1.12-32_arm64.deb ...
Dépaquetage de libusb-0.1-4:arm64 (2:0.1.12-32) ...
Sélection du paquet libftdi1:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../04-libftdi1_0.20-4+b1_arm64.deb ...
Dépaquetage de libftdi1:arm64 (0.20-4+b1) ...
Sélection du paquet libhidapi-libusb0:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../05-libhidapi-libusb0_0.13.1-1_arm64.deb ...
Dépaquetage de libhidapi-libusb0:arm64 (0.13.1-1) ...
Sélection du paquet avrdude précédemment désélectionné.
Préparation du dépaquetage de .../06-avrdude_7.1+dfsg-3_arm64.deb ...
Dépaquetage de avrdude (7.1+dfsg-3) ...
Sélection du paquet binutils-arm-none-eabi précédemment désélectionné.
Préparation du dépaquetage de .../07-binutils-arm-none-eabi_2.40-2+18+b1_arm64.deb ...
Dépaquetage de binutils-arm-none-eabi (2.40-2+18+b1) ...
Sélection du paquet dfu-util précédemment désélectionné.
Préparation du dépaquetage de .../08-dfu-util_0.11-1_arm64.deb ...
Dépaquetage de dfu-util (0.11-1) ...
Sélection du paquet gcc-arm-none-eabi précédemment désélectionné.
Préparation du dépaquetage de .../09-gcc-arm-none-eabi_15%3a12.2.rel1-1_arm64.deb ...
Dépaquetage de gcc-arm-none-eabi (15:12.2.rel1-1) ...
Sélection du paquet libexpat1-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../10-libexpat1-dev_2.5.0-1_arm64.deb ...
Dépaquetage de libexpat1-dev:arm64 (2.5.0-1) ...
Sélection du paquet libffi-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../11-libffi-dev_3.4.4-1_arm64.deb ...
Dépaquetage de libffi-dev:arm64 (3.4.4-1) ...
Sélection du paquet libjs-jquery précédemment désélectionné.
Préparation du dépaquetage de .../12-libjs-jquery_3.6.1+dfsg+~3.5.14-1_all.deb ...
Dépaquetage de libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Sélection du paquet libjs-underscore précédemment désélectionné.
Préparation du dépaquetage de .../13-libjs-underscore_1.13.4~dfsg+~1.11.4-3_all.deb ...
Dépaquetage de libjs-underscore (1.13.4~dfsg+~1.11.4-3) ...
Sélection du paquet libjs-sphinxdoc précédemment désélectionné.
Préparation du dépaquetage de .../14-libjs-sphinxdoc_5.3.0-4_all.deb ...
Dépaquetage de libjs-sphinxdoc (5.3.0-4) ...
Sélection du paquet libncurses6:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../15-libncurses6_6.4-4_arm64.deb ...
Dépaquetage de libncurses6:arm64 (6.4-4) ...
Sélection du paquet libncurses-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../16-libncurses-dev_6.4-4_arm64.deb ...
Dépaquetage de libncurses-dev:arm64 (6.4-4) ...
Sélection du paquet libnewlib-dev précédemment désélectionné.
Préparation du dépaquetage de .../17-libnewlib-dev_3.3.0-1.3_all.deb ...
Dépaquetage de libnewlib-dev (3.3.0-1.3) ...
Sélection du paquet libnewlib-arm-none-eabi précédemment désélectionné.
Préparation du dépaquetage de .../18-libnewlib-arm-none-eabi_3.3.0-1.3_all.deb ...
Dépaquetage de libnewlib-arm-none-eabi (3.3.0-1.3) ...
Sélection du paquet libpkgconf3:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../19-libpkgconf3_1.8.1-1_arm64.deb ...
Dépaquetage de libpkgconf3:arm64 (1.8.1-1) ...
Sélection du paquet libpython3.11:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../20-libpython3.11_3.11.2-6_arm64.deb ...
Dépaquetage de libpython3.11:arm64 (3.11.2-6) ...
Sélection du paquet libpython3.11-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../21-libpython3.11-dev_3.11.2-6_arm64.deb ...
Dépaquetage de libpython3.11-dev:arm64 (3.11.2-6) ...
Sélection du paquet libpython3-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../22-libpython3-dev_3.11.2-1+b1_arm64.deb ...
Dépaquetage de libpython3-dev:arm64 (3.11.2-1+b1) ...
Sélection du paquet libusb-1.0-0-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../23-libusb-1.0-0-dev_2%3a1.0.26-1_arm64.deb ...
Dépaquetage de libusb-1.0-0-dev:arm64 (2:1.0.26-1) ...
Sélection du paquet libusb-1.0-doc précédemment désélectionné.
Préparation du dépaquetage de .../24-libusb-1.0-doc_2%3a1.0.26-1_all.deb ...
Dépaquetage de libusb-1.0-doc (2:1.0.26-1) ...
Sélection du paquet libusb-dev précédemment désélectionné.
Préparation du dépaquetage de .../25-libusb-dev_2%3a0.1.12-32_arm64.deb ...
Dépaquetage de libusb-dev (2:0.1.12-32) ...
Sélection du paquet pkgconf-bin précédemment désélectionné.
Préparation du dépaquetage de .../26-pkgconf-bin_1.8.1-1_arm64.deb ...
Dépaquetage de pkgconf-bin (1.8.1-1) ...
Sélection du paquet pkgconf:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../27-pkgconf_1.8.1-1_arm64.deb ...
Dépaquetage de pkgconf:arm64 (1.8.1-1) ...
Sélection du paquet pkg-config:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../28-pkg-config_1.8.1-1_arm64.deb ...
Dépaquetage de pkg-config:arm64 (1.8.1-1) ...
Sélection du paquet python3.11-dev précédemment désélectionné.
Préparation du dépaquetage de .../29-python3.11-dev_3.11.2-6_arm64.deb ...
Dépaquetage de python3.11-dev (3.11.2-6) ...
Sélection du paquet python3-dev précédemment désélectionné.
Préparation du dépaquetage de .../30-python3-dev_3.11.2-1+b1_arm64.deb ...
Dépaquetage de python3-dev (3.11.2-1+b1) ...
Sélection du paquet python3-distlib précédemment désélectionné.
Préparation du dépaquetage de .../31-python3-distlib_0.3.6-1_all.deb ...
Dépaquetage de python3-distlib (0.3.6-1) ...
Sélection du paquet python3-filelock précédemment désélectionné.
Préparation du dépaquetage de .../32-python3-filelock_3.9.0-1_all.deb ...
Dépaquetage de python3-filelock (3.9.0-1) ...
Sélection du paquet python3-pip-whl précédemment désélectionné.
Préparation du dépaquetage de .../33-python3-pip-whl_23.0.1+dfsg-1_all.deb ...
Dépaquetage de python3-pip-whl (23.0.1+dfsg-1) ...
Sélection du paquet python3-platformdirs précédemment désélectionné.
Préparation du dépaquetage de .../34-python3-platformdirs_2.6.0-1_all.deb ...
Dépaquetage de python3-platformdirs (2.6.0-1) ...
Sélection du paquet python3-setuptools-whl précédemment désélectionné.
Préparation du dépaquetage de .../35-python3-setuptools-whl_66.1.1-1_all.deb ...
Dépaquetage de python3-setuptools-whl (66.1.1-1) ...
Sélection du paquet python3-wheel-whl précédemment désélectionné.
Préparation du dépaquetage de .../36-python3-wheel-whl_0.38.4-2_all.deb ...
Dépaquetage de python3-wheel-whl (0.38.4-2) ...
Sélection du paquet python3-virtualenv précédemment désélectionné.
Préparation du dépaquetage de .../37-python3-virtualenv_20.17.1+ds-1_all.deb ...
Dépaquetage de python3-virtualenv (20.17.1+ds-1) ...
Sélection du paquet stm32flash précédemment désélectionné.
Préparation du dépaquetage de .../38-stm32flash_0.7-1_arm64.deb ...
Dépaquetage de stm32flash (0.7-1) ...
Sélection du paquet virtualenv précédemment désélectionné.
Préparation du dépaquetage de .../39-virtualenv_20.17.1+ds-1_all.deb ...
Dépaquetage de virtualenv (20.17.1+ds-1) ...
Paramétrage de binutils-avr (2.26.20160125+Atmel3.6.2-4) ...
Paramétrage de python3-setuptools-whl (66.1.1-1) ...
Paramétrage de python3-filelock (3.9.0-1) ...
Paramétrage de binutils-arm-none-eabi (2.40-2+18+b1) ...
Paramétrage de libpython3.11:arm64 (3.11.2-6) ...
Paramétrage de python3-pip-whl (23.0.1+dfsg-1) ...
Paramétrage de gcc-arm-none-eabi (15:12.2.rel1-1) ...
Paramétrage de libusb-1.0-doc (2:1.0.26-1) ...
Paramétrage de libusb-1.0-0-dev:arm64 (2:1.0.26-1) ...
Paramétrage de python3-distlib (0.3.6-1) ...
Paramétrage de libffi-dev:arm64 (3.4.4-1) ...
Paramétrage de python3-platformdirs (2.6.0-1) ...
Paramétrage de stm32flash (0.7-1) ...
Paramétrage de libusb-0.1-4:arm64 (2:0.1.12-32) ...
Paramétrage de gcc-avr (1:5.4.0+Atmel3.6.2-3) ...
Paramétrage de libpkgconf3:arm64 (1.8.1-1) ...
Paramétrage de libexpat1-dev:arm64 (2.5.0-1) ...
Paramétrage de libpython3.11-dev:arm64 (3.11.2-6) ...
Paramétrage de libncurses6:arm64 (6.4-4) ...
Paramétrage de libnewlib-dev (3.3.0-1.3) ...
Paramétrage de libhidapi-libusb0:arm64 (0.13.1-1) ...
Paramétrage de pkgconf-bin (1.8.1-1) ...
Paramétrage de libnewlib-arm-none-eabi (3.3.0-1.3) ...
Paramétrage de libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Paramétrage de python3-wheel-whl (0.38.4-2) ...
Paramétrage de dfu-util (0.11-1) ...
Paramétrage de libjs-underscore (1.13.4~dfsg+~1.11.4-3) ...
Paramétrage de libncurses-dev:arm64 (6.4-4) ...
Paramétrage de libpython3-dev:arm64 (3.11.2-1+b1) ...
Paramétrage de python3.11-dev (3.11.2-6) ...
Paramétrage de libftdi1:arm64 (0.20-4+b1) ...
Paramétrage de python3-virtualenv (20.17.1+ds-1) ...
Paramétrage de libusb-dev (2:0.1.12-32) ...
Paramétrage de avr-libc (1:2.0.0+Atmel3.6.2-3) ...
Paramétrage de pkgconf:arm64 (1.8.1-1) ...
Paramétrage de virtualenv (20.17.1+ds-1) ...
Paramétrage de pkg-config:arm64 (1.8.1-1) ...
Paramétrage de libjs-sphinxdoc (5.3.0-4) ...
Paramétrage de avrdude (7.1+dfsg-3) ...
Paramétrage de python3-dev (3.11.2-1+b1) ...
Traitement des actions différées (« triggers ») pour man-db (2.11.2-2) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.36-9+deb12u6) ...
[✓ OK] Klipper packages installed!

###### Installing Python 3.11.2 virtual environment...
created virtual environment CPython3.11.2.final.0-64 in 1948ms
  creator CPython3Posix(dest=/home/mks/klippy-env, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/mks/.local/share/virtualenv)
    added seed packages: pip==23.0.1, setuptools==66.1.1, wheel==0.38.4
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
Requirement already satisfied: pip in /home/mks/klippy-env/lib/python3.11/site-packages (23.0.1)
Collecting pip
  Downloading pip-24.0-py3-none-any.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 4.8 MB/s eta 0:00:00
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 23.0.1
    Uninstalling pip-23.0.1:
      Successfully uninstalled pip-23.0.1
Successfully installed pip-24.0
Ignoring greenlet: markers 'python_version >= "3.12"' don't match your environment
Collecting cffi==1.14.6 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 5))
  Downloading cffi-1.14.6.tar.gz (475 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 475.7/475.7 kB 2.3 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting pyserial==3.4 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 6))
  Downloading pyserial-3.4-py2.py3-none-any.whl.metadata (1.6 kB)
Collecting greenlet==2.0.2 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 7))
  Downloading greenlet-2.0.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (4.1 kB)
Collecting Jinja2==2.11.3 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 9))
  Downloading Jinja2-2.11.3-py2.py3-none-any.whl.metadata (3.5 kB)
Collecting python-can==3.3.4 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 10))
  Downloading python-can-3.3.4.tar.gz (179 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 179.1/179.1 kB 2.5 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting markupsafe==1.1.1 (from -r /home/mks/klipper/scripts/klippy-requirements.txt (line 11))
  Downloading MarkupSafe-1.1.1.tar.gz (19 kB)
  Preparing metadata (setup.py) ... done
Collecting pycparser (from cffi==1.14.6->-r /home/mks/klipper/scripts/klippy-requirements.txt (line 5))
  Downloading pycparser-2.22-py3-none-any.whl.metadata (943 bytes)
Collecting aenum (from python-can==3.3.4->-r /home/mks/klipper/scripts/klippy-requirements.txt (line 10))
  Downloading aenum-3.1.15-py3-none-any.whl.metadata (3.7 kB)
Collecting wrapt~=1.10 (from python-can==3.3.4->-r /home/mks/klipper/scripts/klippy-requirements.txt (line 10))
  Downloading wrapt-1.16.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (6.6 kB)
Downloading pyserial-3.4-py2.py3-none-any.whl (193 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 193.7/193.7 kB 6.4 MB/s eta 0:00:00
Downloading greenlet-2.0.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (612 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 612.4/612.4 kB 6.9 MB/s eta 0:00:00
Downloading Jinja2-2.11.3-py2.py3-none-any.whl (125 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 125.7/125.7 kB 4.7 MB/s eta 0:00:00
Downloading wrapt-1.16.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (80 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 80.9/80.9 kB 3.7 MB/s eta 0:00:00
Downloading aenum-3.1.15-py3-none-any.whl (137 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 137.6/137.6 kB 5.2 MB/s eta 0:00:00
Downloading pycparser-2.22-py3-none-any.whl (117 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 117.6/117.6 kB 4.6 MB/s eta 0:00:00
Building wheels for collected packages: cffi, python-can, markupsafe
  Building wheel for cffi (setup.py) ... done
  Created wheel for cffi: filename=cffi-1.14.6-cp311-cp311-linux_aarch64.whl size=393786 sha256=a0976b6b5aecdc9dd14fdef4899b19e3309ad294431bbf735160449d2f153b9c
  Stored in directory: /home/mks/.cache/pip/wheels/ee/66/dc/d338d6a63e392a6a3d557af5de6a519bf4cf3440a86e7eef74
  Building wheel for python-can (setup.py) ... done
  Created wheel for python-can: filename=python_can-3.3.4-py2.py3-none-any.whl size=154198 sha256=8ea88a8f344dc627f26596d3a7e40656c8d4b4e15d7017da1b7ca7a2fdd49d32
  Stored in directory: /home/mks/.cache/pip/wheels/d3/73/e8/bd33204966a688cf05a620b221a626757a060c41ddcb5054d3
  Building wheel for markupsafe (setup.py) ... done
  Created wheel for markupsafe: filename=MarkupSafe-1.1.1-cp311-cp311-linux_aarch64.whl size=29709 sha256=ccbf132b30def49993ee96d3605a96ef9cf1844fe7b5b37f570df13cae948147
  Stored in directory: /home/mks/.cache/pip/wheels/42/2c/24/ee2a56da668805256e7c1b1d51ae9b177360788cabcdf5b9c1
Successfully built cffi python-can markupsafe
Installing collected packages: pyserial, aenum, wrapt, pycparser, markupsafe, greenlet, python-can, Jinja2, cffi
Successfully installed Jinja2-2.11.3 aenum-3.1.15 cffi-1.14.6 greenlet-2.0.2 markupsafe-1.1.1 pycparser-2.22 pyserial-3.4 python-can-3.3.4 wrapt-1.16.0

###### Creating folder '/home/mks/printer_data/backup' ...
[✓ OK] Folder '/home/mks/printer_data/backup' created!

###### Creating folder '/home/mks/printer_data/certs' ...
[✓ OK] Folder '/home/mks/printer_data/certs' created!

###### Creating folder '/home/mks/printer_data/config' ...
[✓ OK] Folder '/home/mks/printer_data/config' created!

###### Creating folder '/home/mks/printer_data/database' ...
[✓ OK] Folder '/home/mks/printer_data/database' created!

###### Creating folder '/home/mks/printer_data/gcodes' ...
[✓ OK] Folder '/home/mks/printer_data/gcodes' created!

###### Creating folder '/home/mks/printer_data/comms' ...
[✓ OK] Folder '/home/mks/printer_data/comms' created!

###### Creating folder '/home/mks/printer_data/logs' ...
[✓ OK] Folder '/home/mks/printer_data/logs' created!

###### Creating folder '/home/mks/printer_data/systemd' ...
[✓ OK] Folder '/home/mks/printer_data/systemd' created!

###### Create Klipper service file ...
[✓ OK] Klipper service file created!

###### Creating minimal example printer.cfg ...
[✓ OK] Minimal example printer.cfg created!

###### Enable klipper.service ...
Created symlink /etc/systemd/system/multi-user.target.wants/klipper.service → /etc/systemd/system/klipper.service.
[✓ OK] Enable klipper.service successfull!

###### Start klipper.service ...
[✓ OK] Start klipper.service successfull!

###### Installed brltty package detected, masking brltty service ...
[✓ OK] brltty service masked!

###### Installed brltty-udev package detected, masking brltty-udev service ...
[✓ OK] brltty-udev service masked!

###### Installed ModemManager package detected, masking ModemManager service ...
[✓ OK] ModemManager service masked!

#=======================================================#
 Klipper has been set up!
#=======================================================#
```

</details>
 
 ![](../Images/kiauh-klipper-installed.jpg)

 ### MOONRAKER

 Choix de l'option (2) pour installer Moonraker

 Moonraker détecte combien d'instances Klipper ont été installées (ici, une seule) puis prépare son environnement.

<details>

<summary>Installation de Moonraker</summary>

```
/=======================================================\
|     ~~~~~~~~~~~~~~~~~ [ KIAUH ] ~~~~~~~~~~~~~~~~~     |
|        Klipper Installation And Update Helper         |
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~     |
\=======================================================/

###### Initializing Moonraker installation ...
[✓ OK] Klipper installation found!

###### Install Moonraker? (Y/n):
   [➔] Yes

###### Installing Moonraker ...

###### Checking for the following dependencies:
● git
● wget
● curl
● unzip
● dfu-util
● virtualenv
● libjpeg-dev
● zlib1g-dev

###### Installing the following dependencies:
● libjpeg-dev


###### Installing packages...
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Les paquets supplémentaires suivants seront installés :
  libjpeg62-turbo libjpeg62-turbo-dev
Les NOUVEAUX paquets suivants seront installés :
  libjpeg-dev libjpeg62-turbo libjpeg62-turbo-dev
0 mis à jour, 3 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 535 ko dans les archives.
Après cette opération, 1381 ko d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian bookworm/main arm64 libjpeg62-turbo arm64 1:2.1.5-2 [171 kB]
Réception de :2 http://deb.debian.org/debian bookworm/main arm64 libjpeg62-turbo-dev arm64 1:2.1.5-2 [292 kB]
Réception de :3 http://deb.debian.org/debian bookworm/main arm64 libjpeg-dev arm64 1:2.1.5-2 [71,8 kB]
535 ko réceptionnés en 0s (2493 ko/s)
Sélection du paquet libjpeg62-turbo:arm64 précédemment désélectionné.
(Lecture de la base de données... 74863 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libjpeg62-turbo_1%3a2.1.5-2_arm64.deb ...
Dépaquetage de libjpeg62-turbo:arm64 (1:2.1.5-2) ...
Sélection du paquet libjpeg62-turbo-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../libjpeg62-turbo-dev_1%3a2.1.5-2_arm64.deb ...
Dépaquetage de libjpeg62-turbo-dev:arm64 (1:2.1.5-2) ...
Sélection du paquet libjpeg-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../libjpeg-dev_1%3a2.1.5-2_arm64.deb ...
Dépaquetage de libjpeg-dev:arm64 (1:2.1.5-2) ...
Paramétrage de libjpeg62-turbo:arm64 (1:2.1.5-2) ...
Paramétrage de libjpeg62-turbo-dev:arm64 (1:2.1.5-2) ...
Paramétrage de libjpeg-dev:arm64 (1:2.1.5-2) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.36-9+deb12u6) ...
[✓ OK] Dependencies packages installed!

###### Cloning Moonraker from https://github.com/Arksine/moonraker.git ...
Clonage dans '/home/mks/moonraker'...
remote: Enumerating objects: 10747, done.
remote: Counting objects: 100% (3772/3772), done.
remote: Compressing objects: 100% (463/463), done.
remote: Total 10747 (delta 3460), reused 3527 (delta 3307), pack-reused 6975
Réception d'objets: 100% (10747/10747), 4.02 Mio | 4.70 Mio/s, fait.
Résolution des deltas: 100% (7995/7995), fait.

###### Installing dependencies ...

###### Reading dependencies...

python3-virtualenv
python3-dev
liblmdb-dev
libopenjp2-7
libsodium-dev
zlib1g-dev
libjpeg-dev
packagekit
wireless-tools
curl

###### Installing packages...
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
python3-virtualenv est déjà la version la plus récente (20.17.1+ds-1).
python3-virtualenv passé en « installé manuellement ».
python3-dev est déjà la version la plus récente (3.11.2-1+b1).
zlib1g-dev est déjà la version la plus récente (1:1.2.13.dfsg-1).
zlib1g-dev passé en « installé manuellement ».
libjpeg-dev est déjà la version la plus récente (1:2.1.5-2).
packagekit est déjà la version la plus récente (1.2.6-5).
packagekit passé en « installé manuellement ».
wireless-tools est déjà la version la plus récente (30~pre9-14).
curl est déjà la version la plus récente (7.88.1-10+deb12u5).
Les paquets supplémentaires suivants seront installés :
  liblmdb0 libsodium23
Paquets recommandés :
  lmdb-doc
Les NOUVEAUX paquets suivants seront installés :
  liblmdb-dev liblmdb0 libopenjp2-7 libsodium-dev libsodium23
0 mis à jour, 5 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 536 ko dans les archives.
Après cette opération, 1757 ko d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian bookworm/main arm64 liblmdb0 arm64 0.9.24-1 [43,7 kB]
Réception de :2 http://deb.debian.org/debian bookworm/main arm64 liblmdb-dev arm64 0.9.24-1 [60,5 kB]
Réception de :3 http://deb.debian.org/debian bookworm/main arm64 libopenjp2-7 arm64 2.5.0-2 [175 kB]
Réception de :4 http://deb.debian.org/debian bookworm/main arm64 libsodium23 arm64 1.0.18-1 [119 kB]
Réception de :5 http://deb.debian.org/debian bookworm/main arm64 libsodium-dev arm64 1.0.18-1 [137 kB]
536 ko réceptionnés en 0s (1962 ko/s)
Sélection du paquet liblmdb0:arm64 précédemment désélectionné.
(Lecture de la base de données... 74898 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../liblmdb0_0.9.24-1_arm64.deb ...
Dépaquetage de liblmdb0:arm64 (0.9.24-1) ...
Sélection du paquet liblmdb-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../liblmdb-dev_0.9.24-1_arm64.deb ...
Dépaquetage de liblmdb-dev:arm64 (0.9.24-1) ...
Sélection du paquet libopenjp2-7:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../libopenjp2-7_2.5.0-2_arm64.deb ...
Dépaquetage de libopenjp2-7:arm64 (2.5.0-2) ...
Sélection du paquet libsodium23:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../libsodium23_1.0.18-1_arm64.deb ...
Dépaquetage de libsodium23:arm64 (1.0.18-1) ...
Sélection du paquet libsodium-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../libsodium-dev_1.0.18-1_arm64.deb ...
Dépaquetage de libsodium-dev:arm64 (1.0.18-1) ...
Paramétrage de liblmdb0:arm64 (0.9.24-1) ...
Paramétrage de libsodium23:arm64 (1.0.18-1) ...
Paramétrage de libsodium-dev:arm64 (1.0.18-1) ...
Paramétrage de liblmdb-dev:arm64 (0.9.24-1) ...
Paramétrage de libopenjp2-7:arm64 (2.5.0-2) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.36-9+deb12u6) ...
[✓ OK] Moonraker packages installed!

###### Installing python virtual environment...
created virtual environment CPython3.11.2.final.0-64 in 997ms
  creator CPython3Posix(dest=/home/mks/moonraker-env, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/mks/.local/share/virtualenv)
    added seed packages: pip==23.0.1, setuptools==66.1.1, wheel==0.38.4
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
Requirement already satisfied: pip in ./moonraker-env/lib/python3.11/site-packages (23.0.1)
Collecting pip
  Using cached pip-24.0-py3-none-any.whl (2.1 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 23.0.1
    Uninstalling pip-23.0.1:
      Successfully uninstalled pip-23.0.1
Successfully installed pip-24.0
Looking in links: /home/mks/moonraker/scripts/python_wheels
Ignoring tornado: markers 'python_version == "3.7"' don't match your environment
Ignoring pillow: markers 'python_version == "3.7"' don't match your environment
Ignoring streaming-form-data: markers 'python_version == "3.7"' don't match your environment
Collecting tornado==6.4.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 4))
  Downloading tornado-6.4-cp38-abi3-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (2.5 kB)
Collecting pyserial==3.4 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 5))
  Using cached pyserial-3.4-py2.py3-none-any.whl.metadata (1.6 kB)
Collecting pyserial-asyncio==0.6 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 6))
  Downloading pyserial_asyncio-0.6-py3-none-any.whl.metadata (1.3 kB)
Collecting pillow==10.3.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 8))
  Downloading pillow-10.3.0-cp311-cp311-manylinux_2_28_aarch64.whl.metadata (9.2 kB)
Collecting lmdb==1.4.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 9))
  Downloading lmdb-1.4.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (1.2 kB)
Collecting streaming-form-data==1.13.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 11))
  Downloading streaming-form-data-1.13.0.tar.gz (95 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 95.0/95.0 kB 1.4 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting distro==1.9.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 12))
  Downloading distro-1.9.0-py3-none-any.whl.metadata (6.8 kB)
Collecting inotify-simple==1.3.5 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 13))
  Downloading inotify_simple-1.3.5.tar.gz (9.7 kB)
  Preparing metadata (setup.py) ... done
Collecting libnacl==2.1.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 14))
  Downloading libnacl-2.1.0-py3-none-any.whl.metadata (3.0 kB)
Collecting paho-mqtt==1.6.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 15))
  Downloading paho-mqtt-1.6.1.tar.gz (99 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 99.4/99.4 kB 4.3 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Collecting zeroconf==0.131.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 16))
  Downloading zeroconf-0.131.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (6.1 kB)
Collecting preprocess-cancellation==0.2.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 17))
  Downloading preprocess_cancellation-0.2.1-py3-none-any.whl.metadata (3.6 kB)
Collecting jinja2==3.1.3 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 18))
  Downloading Jinja2-3.1.3-py3-none-any.whl.metadata (3.3 kB)
Collecting dbus-next==0.2.3 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 19))
  Downloading dbus_next-0.2.3-py3-none-any.whl.metadata (7.0 kB)
Collecting apprise==1.7.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading apprise-1.7.1-py3-none-any.whl.metadata (44 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 44.0/44.0 kB 2.0 MB/s eta 0:00:00
Collecting ldap3==2.9.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 21))
  Downloading ldap3-2.9.1-py2.py3-none-any.whl.metadata (5.4 kB)
Collecting python-periphery==2.4.1 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 22))
  Downloading python_periphery-2.4.1-py2.py3-none-any.whl.metadata (1.5 kB)
Collecting smart_open<=6.4.0 (from -r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 23))
  Downloading smart_open-6.4.0-py3-none-any.whl.metadata (21 kB)
Collecting ifaddr>=0.1.7 (from zeroconf==0.131.0->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 16))
  Downloading ifaddr-0.2.0-py3-none-any.whl.metadata (4.9 kB)
Collecting MarkupSafe>=2.0 (from jinja2==3.1.3->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 18))
  Downloading MarkupSafe-2.1.5-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (3.0 kB)
Collecting certifi (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading certifi-2024.2.2-py3-none-any.whl.metadata (2.2 kB)
Collecting requests (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading requests-2.31.0-py3-none-any.whl.metadata (4.6 kB)
Collecting requests-oauthlib (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading requests_oauthlib-2.0.0-py2.py3-none-any.whl.metadata (11 kB)
Collecting click>=5.0 (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Collecting markdown (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading Markdown-3.6-py3-none-any.whl.metadata (7.0 kB)
Collecting PyYAML (from apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading PyYAML-6.0.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (2.1 kB)
Collecting pyasn1>=0.4.6 (from ldap3==2.9.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 21))
  Downloading pyasn1-0.6.0-py2.py3-none-any.whl.metadata (8.3 kB)
Collecting charset-normalizer<4,>=2 (from requests->apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading charset_normalizer-3.3.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (33 kB)
Collecting idna<4,>=2.5 (from requests->apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading idna-3.7-py3-none-any.whl.metadata (9.9 kB)
Collecting urllib3<3,>=1.21.1 (from requests->apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading urllib3-2.2.1-py3-none-any.whl.metadata (6.4 kB)
Collecting oauthlib>=3.0.0 (from requests-oauthlib->apprise==1.7.1->-r /home/mks/moonraker/scripts/moonraker-requirements.txt (line 20))
  Downloading oauthlib-3.2.2-py3-none-any.whl.metadata (7.5 kB)
Downloading tornado-6.4-cp38-abi3-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (435 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 435.9/435.9 kB 5.7 MB/s eta 0:00:00
Using cached pyserial-3.4-py2.py3-none-any.whl (193 kB)
Downloading pyserial_asyncio-0.6-py3-none-any.whl (7.6 kB)
Downloading pillow-10.3.0-cp311-cp311-manylinux_2_28_aarch64.whl (4.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.3/4.3 MB 6.4 MB/s eta 0:00:00
Downloading lmdb-1.4.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (301 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 301.6/301.6 kB 4.6 MB/s eta 0:00:00
Downloading distro-1.9.0-py3-none-any.whl (20 kB)
Downloading libnacl-2.1.0-py3-none-any.whl (21 kB)
Downloading zeroconf-0.131.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (11.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 11.3/11.3 MB 5.9 MB/s eta 0:00:00
Downloading preprocess_cancellation-0.2.1-py3-none-any.whl (18 kB)
Downloading Jinja2-3.1.3-py3-none-any.whl (133 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.2/133.2 kB 4.8 MB/s eta 0:00:00
Downloading dbus_next-0.2.3-py3-none-any.whl (57 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 57.9/57.9 kB 2.5 MB/s eta 0:00:00
Downloading apprise-1.7.1-py3-none-any.whl (1.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 6.7 MB/s eta 0:00:00
Downloading ldap3-2.9.1-py2.py3-none-any.whl (432 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 432.2/432.2 kB 5.9 MB/s eta 0:00:00
Downloading python_periphery-2.4.1-py2.py3-none-any.whl (36 kB)
Downloading smart_open-6.4.0-py3-none-any.whl (57 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 57.0/57.0 kB 2.4 MB/s eta 0:00:00
Downloading click-8.1.7-py3-none-any.whl (97 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 4.1 MB/s eta 0:00:00
Downloading ifaddr-0.2.0-py3-none-any.whl (12 kB)
Downloading MarkupSafe-2.1.5-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (29 kB)
Downloading pyasn1-0.6.0-py2.py3-none-any.whl (85 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 85.3/85.3 kB 3.4 MB/s eta 0:00:00
Downloading certifi-2024.2.2-py3-none-any.whl (163 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 163.8/163.8 kB 3.6 MB/s eta 0:00:00
Downloading Markdown-3.6-py3-none-any.whl (105 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 105.4/105.4 kB 4.2 MB/s eta 0:00:00
Downloading PyYAML-6.0.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (732 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 732.2/732.2 kB 5.6 MB/s eta 0:00:00
Downloading requests-2.31.0-py3-none-any.whl (62 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 62.6/62.6 kB 2.6 MB/s eta 0:00:00
Downloading requests_oauthlib-2.0.0-py2.py3-none-any.whl (24 kB)
Downloading charset_normalizer-3.3.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (136 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 136.6/136.6 kB 4.9 MB/s eta 0:00:00
Downloading idna-3.7-py3-none-any.whl (66 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 66.8/66.8 kB 3.0 MB/s eta 0:00:00
Downloading oauthlib-3.2.2-py3-none-any.whl (151 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 151.7/151.7 kB 5.5 MB/s eta 0:00:00
Downloading urllib3-2.2.1-py3-none-any.whl (121 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 121.1/121.1 kB 4.6 MB/s eta 0:00:00
Building wheels for collected packages: streaming-form-data, inotify-simple, paho-mqtt
  Building wheel for streaming-form-data (setup.py) ... done
  Created wheel for streaming-form-data: filename=streaming_form_data-1.13.0-cp311-cp311-linux_aarch64.whl size=355800 sha256=55da5955dde096c2b1d2e0714552afa492b82b28165af611e6118f48b9c8348c
  Stored in directory: /home/mks/.cache/pip/wheels/d4/97/18/9025a411325fdb035145d6c4c8bafaed4b0ce325540f094d18
  Building wheel for inotify-simple (setup.py) ... done
  Created wheel for inotify-simple: filename=inotify_simple-1.3.5-py3-none-any.whl size=7689 sha256=d5b38900167d59f95f04adb595f1fae23ce15a85873197accf0f644b6c2466ad
  Stored in directory: /home/mks/.cache/pip/wheels/62/4d/7f/e5e1cee8d11c6cd5bde3b7a60afe4498e46123173d92dd981c
  Building wheel for paho-mqtt (setup.py) ... done
  Created wheel for paho-mqtt: filename=paho_mqtt-1.6.1-py3-none-any.whl size=62118 sha256=57545148afee0afbea31576f34cc6a2ff6620d0e0abbcb6cab8d2d6394e77ca0
  Stored in directory: /home/mks/.cache/pip/wheels/29/ea/a5/ba9a63aaf4cd4e16e8a87ee31fb4d11b04ff5e1735d312619a
Successfully built streaming-form-data inotify-simple paho-mqtt
Installing collected packages: python-periphery, pyserial, paho-mqtt, lmdb, ifaddr, zeroconf, urllib3, tornado, smart_open, PyYAML, pyserial-asyncio, pyasn1, preprocess-cancellation, pillow, oauthlib, MarkupSafe, markdown, libnacl, inotify-simple, idna, distro, dbus-next, click, charset-normalizer, certifi, streaming-form-data, requests, ldap3, jinja2, requests-oauthlib, apprise
Successfully installed MarkupSafe-2.1.5 PyYAML-6.0.1 apprise-1.7.1 certifi-2024.2.2 charset-normalizer-3.3.2 click-8.1.7 dbus-next-0.2.3 distro-1.9.0 idna-3.7 ifaddr-0.2.0 inotify-simple-1.3.5 jinja2-3.1.3 ldap3-2.9.1 libnacl-2.1.0 lmdb-1.4.1 markdown-3.6 oauthlib-3.2.2 paho-mqtt-1.6.1 pillow-10.3.0 preprocess-cancellation-0.2.1 pyasn1-0.6.0 pyserial-3.4 pyserial-asyncio-0.6 python-periphery-2.4.1 requests-2.31.0 requests-oauthlib-2.0.0 smart_open-6.4.0 streaming-form-data-1.13.0 tornado-6.4 urllib3-2.2.1 zeroconf-0.131.0

###### Creating moonraker.conf in /home/mks/printer_data/config ...
[✓ OK] moonraker.conf created!

###### Creating Moonraker Service  ...
[✓ OK] Moonraker instance created!

###### Installing Moonraker policykit rules ...


###### PolicyKit Version 122 Detected


###### Installing PolicyKit Rules to /usr/share/polkit-1/rules.d/moonraker.rules...


###### Restarting Moonraker...
[✓ OK] Moonraker policykit rules installed!

###### Enable moonraker.service ...
Created symlink /etc/systemd/system/multi-user.target.wants/moonraker.service → /etc/systemd/system/moonraker.service.
[✓ OK] Enable moonraker.service successfull!

###### Start moonraker.service ...
[✓ OK] Start moonraker.service successfull!

#=======================================================#
 Moonraker has been set up!
#=======================================================#

   ● Instance 1: 192.168.1.126:7125
```

</details>

![](../Images/kiauh-moonraker-installed.jpg)

### Interfaces d'accès Web

Interfaces Web permettant l'accès à partir d'un navigateur Internet à Klipper via les APIs de Moonraker choix (3) pour Mainsail et (4) pour Fluidd. 

On peut n'en installer qu'une des deux, l'installation de la première, par défaut, communiquera avec le port HTTP standard (80). Pour la seconde, il faudra fournir un autre numéro de port.

#### MAINSAIL

Lors de cette première installation, répondre Yes (Oui) à la demande d'installation des macros «client» (PAUSE, RESUME, …)


<details>
 <sumarry>Installation de Mainsail</sumarry>

```
/=======================================================\
|     ~~~~~~~~~~~~~~~~~ [ KIAUH ] ~~~~~~~~~~~~~~~~~     |
|        Klipper Installation And Update Helper         |
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~     |
\=======================================================/

###### Checking for the following dependencies:
● wget
● nginx

###### Installing the following dependencies:
● nginx


###### Installing packages...
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Les paquets supplémentaires suivants seront installés :
  nginx-common
Paquets suggérés :
  fcgiwrap nginx-doc ssl-cert
Les NOUVEAUX paquets suivants seront installés :
  nginx nginx-common
0 mis à jour, 2 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 606 ko dans les archives.
Après cette opération, 1708 ko d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian bookworm/main arm64 nginx-common all 1.22.1-9 [112 kB]
Réception de :2 http://deb.debian.org/debian bookworm/main arm64 nginx arm64 1.22.1-9 [494 kB]
606 ko réceptionnés en 0s (2253 ko/s)
Preconfiguring packages ...
Sélection du paquet nginx-common précédemment désélectionné.
(Lecture de la base de données... 74996 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../nginx-common_1.22.1-9_all.deb ...
Dépaquetage de nginx-common (1.22.1-9) ...
Sélection du paquet nginx précédemment désélectionné.
Préparation du dépaquetage de .../nginx_1.22.1-9_arm64.deb ...
Dépaquetage de nginx (1.22.1-9) ...
Paramétrage de nginx-common (1.22.1-9) ...
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /lib/systemd/system/nginx.service.
Paramétrage de nginx (1.22.1-9) ...
Upgrading binary: nginx.
Traitement des actions différées (« triggers ») pour man-db (2.11.2-2) ...
[✓ OK] Dependencies packages installed!

###### Initializing Mainsail installation ...

###### Create KIAUH backup directory ...
[✓ OK] Directory created!

###### Timestamp: 240501-1719

###### Create backup of /home/mks/printer_data/config ...
[✓ OK] Backup created in:
/home/mks/kiauh-backups/configs/240501-1719/printer_data

###### Downloading Mainsail from https://github.com/mainsail-crew/mainsail/releases/latest/download/mainsail.zip ...
--2024-05-01 17:19:02--  https://github.com/mainsail-crew/mainsail/releases/latest/download/mainsail.zip
Résolution de github.com (github.com)… 140.82.121.3
Connexion à github.com (github.com)|140.82.121.3|:443… connecté.
requête HTTP transmise, en attente de la réponse… 302 Found
Emplacement : https://github.com/mainsail-crew/mainsail/releases/download/v2.11.1/mainsail.zip [suivant]
--2024-05-01 17:19:02--  https://github.com/mainsail-crew/mainsail/releases/download/v2.11.1/mainsail.zip
Réutilisation de la connexion existante à github.com:443.
requête HTTP transmise, en attente de la réponse… 302 Found
Emplacement : https://objects.githubusercontent.com/github-production-release-asset-2e65be/240875926/9cc07641-5ed4-4195-ba80-2c13aefcd6c5?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240501%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240501T151902Z&X-Amz-Expires=300&X-Amz-Signature=0bdf2e3c08915e29b1d1d8a4b89d20ce5ac4c3c9b2092134dcd7844377d5d420&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=240875926&response-content-disposition=attachment%3B%20filename%3Dmainsail.zip&response-content-type=application%2Foctet-stream [suivant]
--2024-05-01 17:19:02--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/240875926/9cc07641-5ed4-4195-ba80-2c13aefcd6c5?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240501%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240501T151902Z&X-Amz-Expires=300&X-Amz-Signature=0bdf2e3c08915e29b1d1d8a4b89d20ce5ac4c3c9b2092134dcd7844377d5d420&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=240875926&response-content-disposition=attachment%3B%20filename%3Dmainsail.zip&response-content-type=application%2Foctet-stream
Résolution de objects.githubusercontent.com (objects.githubusercontent.com)… 185.199.111.133, 185.199.110.133, 185.199.108.133, ...
Connexion à objects.githubusercontent.com (objects.githubusercontent.com)|185.199.111.133|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 2114772 (2,0M) [application/octet-stream]
Sauvegarde en : « mainsail.zip »

mainsail.zip        100%[===================>]   2,02M  9,79MB/s    ds 0,2s

2024-05-01 17:19:03 (9,79 MB/s) — « mainsail.zip » sauvegardé [2114772/2114772]

[✓ OK] Download complete!

###### Extracting archive ...
[✓ OK] Done!

###### Remove downloaded archive ...
[✓ OK] Done!

/=======================================================\
| It is recommended to use special macros in order to   |
| have Mainsail fully functional and working.           |
|                                                       |
| The recommended macros for Mainsail can be seen here: |
| https://github.com/mainsail-crew/mainsail-config      |
|                                                       |
| If you already use these macros skip this step.       |
| Otherwise you should consider to answer with 'yes' to |
| download the recommended macros.                      |
\=======================================================/
###### Download the recommended macros? (Y/n):
   [➔] Yes

###### Cloning mainsail-config ...
Clonage dans '/home/mks/mainsail-config'...
remote: Enumerating objects: 89, done.
remote: Counting objects: 100% (56/56), done.
remote: Compressing objects: 100% (35/35), done.
remote: Total 89 (delta 51), reused 21 (delta 21), pack-reused 33
Réception d'objets: 100% (89/89), 43.76 Kio | 1.68 Mio/s, fait.
Résolution des deltas: 100% (52/52), fait.

###### Adding Mainsail-Config to update manager in file:
       /home/mks/printer_data/config/moonraker.conf

###### Restart moonraker.service ...
[✓ OK] Restart moonraker.service successfull!
[✓ OK] Done!

###### Checking for the following dependencies:
● nginx
[✓ OK] Dependencies already met!

###### Creating NGINX configuration for Mainsail ...
[✓ OK] Mainsail configured for port 80!
[✓ OK] NGINX configuration for Mainsail was set!

###### Granting NGINX the required permissions ...
[✓ OK] Done!

###### Creating symlink for /var/log/nginx/mainsail-access.log ...
[✓ OK] Symlink created: /home/mks/printer_data/logs/mainsail-access.log

###### Creating symlink for /var/log/nginx/mainsail-error.log ...
[✓ OK] Symlink created: /home/mks/printer_data/logs/mainsail-error.log

###### Adding Mainsail to update manager in file:
       /home/mks/printer_data/config/moonraker.conf

###### Restart moonraker.service ...
[✓ OK] Restart moonraker.service successfull!

#=======================================================#
 Mainsail has been set up!
#=======================================================#
``` 

</details>

#### FLUIDD

Mainsail (si installé en premier) utilise le port standard HTTP (80), il est nécessaire d'en indiquer un différent pour l'accès à Fluidd (Qidi Tech utilise le port 10088).

Normalement, si Mainsail a déjà été installé, les macros (client.cfg) indispensables ont elles-aussi été ajoutées, inutile de les réinstaller pour Fluidd (répondre Non à la demande d'installation). Ces deux interfaces Web utilisent désormais les mêmes macros (PAUSE, RESUME. …).


<details>
 <sumarry>Installation de Fluidd</sumarry>

```
/=======================================================\
|     ~~~~~~~~~~~~~~~~~ [ KIAUH ] ~~~~~~~~~~~~~~~~~     |
|        Klipper Installation And Update Helper         |
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~     |
\=======================================================/

###### Checking for the following dependencies:
● wget
● nginx
[✓ OK] Dependencies already met!

###### Initializing Fluidd installation ...

###### Timestamp: 240501-1720

###### Create backup of /home/mks/printer_data/config ...
[✓ OK] Backup created in:
/home/mks/kiauh-backups/configs/240501-1720/printer_data

###### Detected other enabled interfaces:
   ● Mainsail - Port: 80

/=======================================================\
|                    !!!WARNING!!!                      |
| You need to choose a different port for Fluidd!       |
| The following web interface is listening at port 80:  |
|                                                       |
|  ● Mainsail                                           |
|                                                       |
| Make sure you don't choose a port which was already   |
| assigned to another webinterface!                     |
|                                                       |
| Be aware: there is NO sanity check for the following  |
| input. So make sure to choose a valid port!           |
\=======================================================/
Please enter a new Port: 10088
   [➔] Setting port 10088 for Fluidd!

###### Downloading Fluidd from https://github.com/fluidd-core/fluidd/releases/latest/download/fluidd.zip ...
--2024-05-01 17:20:42--  https://github.com/fluidd-core/fluidd/releases/latest/download/fluidd.zip
Résolution de github.com (github.com)… 140.82.121.3
Connexion à github.com (github.com)|140.82.121.3|:443… connecté.
requête HTTP transmise, en attente de la réponse… 302 Found
Emplacement : https://github.com/fluidd-core/fluidd/releases/download/v1.29.1/fluidd.zip [suivant]
--2024-05-01 17:20:42--  https://github.com/fluidd-core/fluidd/releases/download/v1.29.1/fluidd.zip
Réutilisation de la connexion existante à github.com:443.
requête HTTP transmise, en attente de la réponse… 302 Found
Emplacement : https://objects.githubusercontent.com/github-production-release-asset-2e65be/295836951/71aafabe-9e9a-4a9f-b9a5-2e09cd260505?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240501%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240501T152042Z&X-Amz-Expires=300&X-Amz-Signature=299bc438661e5cadaa68d3ce18bc341ed7a4eeabafe973d269bcb6c5f53515a3&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=295836951&response-content-disposition=attachment%3B%20filename%3Dfluidd.zip&response-content-type=application%2Foctet-stream [suivant]
--2024-05-01 17:20:42--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/295836951/71aafabe-9e9a-4a9f-b9a5-2e09cd260505?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240501%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240501T152042Z&X-Amz-Expires=300&X-Amz-Signature=299bc438661e5cadaa68d3ce18bc341ed7a4eeabafe973d269bcb6c5f53515a3&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=295836951&response-content-disposition=attachment%3B%20filename%3Dfluidd.zip&response-content-type=application%2Foctet-stream
Résolution de objects.githubusercontent.com (objects.githubusercontent.com)… 185.199.109.133, 185.199.108.133, 185.199.110.133, ...
Connexion à objects.githubusercontent.com (objects.githubusercontent.com)|185.199.109.133|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 3684373 (3,5M) [application/octet-stream]
Sauvegarde en : « fluidd.zip »

fluidd.zip          100%[===================>]   3,51M  9,63MB/s    ds 0,4s

2024-05-01 17:20:43 (9,63 MB/s) — « fluidd.zip » sauvegardé [3684373/3684373]

[✓ OK] Download complete!

###### Extracting archive ...
[✓ OK] Done!

###### Remove downloaded archive ...
[✓ OK] Done!

/=======================================================\
| It is recommended to use special macros in order to   |
| have Fluidd fully functional and working.             |
|                                                       |
| The recommended macros for Fluidd can be found here:  |
| https://github.com/fluidd-core/fluidd-config           |
|                                                       |
| If you already use these macros skip this step.       |
| Otherwise you should consider to answer with 'yes' to |
| download the recommended macros.                      |
\=======================================================/
###### Download the recommended macros? (Y/n): n
   [➔] No

###### Checking for the following dependencies:
● nginx
[✓ OK] Dependencies already met!

###### Creating NGINX configuration for Fluidd ...
[✓ OK] Fluidd configured for port 10088!
[✓ OK] NGINX configuration for Fluidd was set!

###### Granting NGINX the required permissions ...
[✓ OK] Done!

###### Creating symlink for /var/log/nginx/fluidd-access.log ...
[✓ OK] Symlink created: /home/mks/printer_data/logs/fluidd-access.log

###### Creating symlink for /var/log/nginx/fluidd-error.log ...
[✓ OK] Symlink created: /home/mks/printer_data/logs/fluidd-error.log

###### Adding Fluidd to update manager in file:
       /home/mks/printer_data/config/moonraker.conf

###### Restart moonraker.service ...
[✓ OK] Restart moonraker.service successfull!

#=======================================================#
 Fluidd has been set up!
#=======================================================#
``` 

</details>

### CROWSNEST

Crowsnest est responsable de la configuration et du contrôle des webcams (même en l'absence de caméra, autant l'installer de suite).

<details>

  <sumarry>Installation de Crowsnest</sumarry>

```
/=======================================================\
|     ~~~~~~~~~~~~~~~~~ [ KIAUH ] ~~~~~~~~~~~~~~~~~     |
|        Klipper Installation And Update Helper         |
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~     |
\=======================================================/

###### Cloning 'crowsnest' repository ...
Clonage dans '/home/mks/crowsnest'...
remote: Enumerating objects: 1698, done.
remote: Counting objects: 100% (320/320), done.
remote: Compressing objects: 100% (163/163), done.
remote: Total 1698 (delta 197), reused 235 (delta 150), pack-reused 1378
Réception d'objets: 100% (1698/1698), 656.79 Kio | 3.08 Mio/s, fait.
Résolution des deltas: 100% (1021/1021), fait.

###### Checking for the following dependencies:
● git
● make
[✓ OK] Dependencies already met!
Installer will prompt you for sudo password!

###### Launching crowsnest installer ...
crowsnest - A webcam daemon for multiple cams and stream services.

        Ahoy!
        Thank you for installing crowsnest ;)
        This will take a while ...
        Please reboot after the installation has finished.

Running apt-get update first ...

Atteint :1 http://security.debian.org bookworm-security InRelease
Atteint :2 http://deb.debian.org/debian bookworm InRelease
Réception de :3 http://deb.debian.org/debian bookworm-updates InRelease [55,4 kB]
Réception de :4 http://deb.debian.org/debian bookworm-backports InRelease [56,5 kB]
Atteint :5 http://fi.mirror.armbian.de/apt bookworm InRelease
Réception de :6 http://deb.debian.org/debian bookworm-backports/main arm64 Packages.diff/Index [63,3 kB]
Réception de :7 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb).diff/Index [63,4 kB]
Réception de :8 http://deb.debian.org/debian bookworm-backports/main all Contents (deb).diff/Index [63,6 kB]
Réception de :9 http://deb.debian.org/debian bookworm-backports/main arm64 Packages T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [3429 B]
Réception de :10 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb) T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [401 B]
Réception de :9 http://deb.debian.org/debian bookworm-backports/main arm64 Packages T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [3429 B]
Réception de :10 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb) T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [401 B]
Réception de :11 http://deb.debian.org/debian bookworm-backports/main all Contents (deb) T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [579 B]
Réception de :11 http://deb.debian.org/debian bookworm-backports/main all Contents (deb) T-2024-05-01-1413.56-F-2024-05-01-1413.56.pdiff [579 B]
307 ko réceptionnés en 10s (29,6 ko/s)
Lecture des listes de paquets…
Running apt-get update first ... [OK]
Doing some tests ...

Checking for mjpg-streamer ...

Checking for mjpg-streamer ... [OK]
Checking for camera-streamer dependencies ...

Checking if device is a Raspberry Pi ...

Checking if device is a Raspberry Pi ... [FAILED]
This device is not a Raspberry Pi therefore camera-streeamer cannot be installed ...
Doing some tests ... [OK]
Installing without camera-streamer ...

Installing dependencies ...

Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
git est déjà la version la plus récente (1:2.39.2-1.1).
bsdutils est déjà la version la plus récente (1:2.38.1-5+deb12u1).
findutils est déjà la version la plus récente (4.9.0-4).
curl est déjà la version la plus récente (7.88.1-10+deb12u5).
build-essential est déjà la version la plus récente (12.9).
libjpeg-dev est déjà la version la plus récente (1:2.1.5-2).
Les paquets supplémentaires suivants seront installés :
  libevent-2.1-7 libevent-extra-2.1-7 libevent-openssl-2.1-7
  libevent-pthreads-2.1-7 libmd-dev libv4l-0 libv4l2rds0 libv4lconvert0
  python3-iniparse
Les NOUVEAUX paquets suivants seront installés :
  crudini libbsd-dev libevent-2.1-7 libevent-dev libevent-extra-2.1-7
  libevent-openssl-2.1-7 libevent-pthreads-2.1-7 libmd-dev libv4l-0
  libv4l2rds0 libv4lconvert0 python3-iniparse v4l-utils
0 mis à jour, 13 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 1953 ko dans les archives.
Après cette opération, 8080 ko d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian bookworm/main arm64 python3-iniparse all 0.5-1 [21,9 kB]
Réception de :2 http://deb.debian.org/debian bookworm/main arm64 crudini arm64 0.9.4-1 [18,6 kB]
Réception de :3 http://deb.debian.org/debian bookworm/main arm64 libmd-dev arm64 1.0.4-2 [47,4 kB]
Réception de :4 http://deb.debian.org/debian bookworm/main arm64 libbsd-dev arm64 0.11.7-2 [242 kB]
Réception de :5 http://deb.debian.org/debian bookworm/main arm64 libevent-2.1-7 arm64 2.1.12-stable-8 [168 kB]
Réception de :6 http://deb.debian.org/debian bookworm/main arm64 libevent-extra-2.1-7 arm64 2.1.12-stable-8 [101 kB]
Réception de :7 http://deb.debian.org/debian bookworm/main arm64 libevent-pthreads-2.1-7 arm64 2.1.12-stable-8 [53,7 kB]
Réception de :8 http://deb.debian.org/debian bookworm/main arm64 libevent-openssl-2.1-7 arm64 2.1.12-stable-8 [59,7 kB]
Réception de :9 http://deb.debian.org/debian bookworm/main arm64 libevent-dev arm64 2.1.12-stable-8 [302 kB]
Réception de :10 http://deb.debian.org/debian bookworm/main arm64 libv4lconvert0 arm64 1.22.1-5+b2 [141 kB]
Réception de :11 http://deb.debian.org/debian bookworm/main arm64 libv4l-0 arm64 1.22.1-5+b2 [107 kB]
Réception de :12 http://deb.debian.org/debian bookworm/main arm64 libv4l2rds0 arm64 1.22.1-5+b2 [80,3 kB]
Réception de :13 http://deb.debian.org/debian bookworm/main arm64 v4l-utils arm64 1.22.1-5+b2 [610 kB]
1953 ko réceptionnés en 0s (4501 ko/s)
Sélection du paquet python3-iniparse précédemment désélectionné.
(Lecture de la base de données... 75051 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../00-python3-iniparse_0.5-1_all.deb ...
Dépaquetage de python3-iniparse (0.5-1) ...
Sélection du paquet crudini précédemment désélectionné.
Préparation du dépaquetage de .../01-crudini_0.9.4-1_arm64.deb ...
Dépaquetage de crudini (0.9.4-1) ...
Sélection du paquet libmd-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../02-libmd-dev_1.0.4-2_arm64.deb ...
Dépaquetage de libmd-dev:arm64 (1.0.4-2) ...
Sélection du paquet libbsd-dev:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../03-libbsd-dev_0.11.7-2_arm64.deb ...
Dépaquetage de libbsd-dev:arm64 (0.11.7-2) ...
Sélection du paquet libevent-2.1-7:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../04-libevent-2.1-7_2.1.12-stable-8_arm64.deb ...
Dépaquetage de libevent-2.1-7:arm64 (2.1.12-stable-8) ...
Sélection du paquet libevent-extra-2.1-7:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../05-libevent-extra-2.1-7_2.1.12-stable-8_arm64.deb ...
Dépaquetage de libevent-extra-2.1-7:arm64 (2.1.12-stable-8) ...
Sélection du paquet libevent-pthreads-2.1-7:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../06-libevent-pthreads-2.1-7_2.1.12-stable-8_arm64.deb ...
Dépaquetage de libevent-pthreads-2.1-7:arm64 (2.1.12-stable-8) ...
Sélection du paquet libevent-openssl-2.1-7:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../07-libevent-openssl-2.1-7_2.1.12-stable-8_arm64.deb ...
Dépaquetage de libevent-openssl-2.1-7:arm64 (2.1.12-stable-8) ...
Sélection du paquet libevent-dev précédemment désélectionné.
Préparation du dépaquetage de .../08-libevent-dev_2.1.12-stable-8_arm64.deb ...
Dépaquetage de libevent-dev (2.1.12-stable-8) ...
Sélection du paquet libv4lconvert0:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../09-libv4lconvert0_1.22.1-5+b2_arm64.deb ...
Dépaquetage de libv4lconvert0:arm64 (1.22.1-5+b2) ...
Sélection du paquet libv4l-0:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../10-libv4l-0_1.22.1-5+b2_arm64.deb ...
Dépaquetage de libv4l-0:arm64 (1.22.1-5+b2) ...
Sélection du paquet libv4l2rds0:arm64 précédemment désélectionné.
Préparation du dépaquetage de .../11-libv4l2rds0_1.22.1-5+b2_arm64.deb ...
Dépaquetage de libv4l2rds0:arm64 (1.22.1-5+b2) ...
Sélection du paquet v4l-utils précédemment désélectionné.
Préparation du dépaquetage de .../12-v4l-utils_1.22.1-5+b2_arm64.deb ...
Dépaquetage de v4l-utils (1.22.1-5+b2) ...
Paramétrage de libevent-extra-2.1-7:arm64 (2.1.12-stable-8) ...
Paramétrage de libevent-openssl-2.1-7:arm64 (2.1.12-stable-8) ...
Paramétrage de libevent-2.1-7:arm64 (2.1.12-stable-8) ...
Paramétrage de libv4lconvert0:arm64 (1.22.1-5+b2) ...
Paramétrage de libmd-dev:arm64 (1.0.4-2) ...
Paramétrage de libv4l2rds0:arm64 (1.22.1-5+b2) ...
Paramétrage de libbsd-dev:arm64 (0.11.7-2) ...
Paramétrage de python3-iniparse (0.5-1) ...
Paramétrage de libevent-pthreads-2.1-7:arm64 (2.1.12-stable-8) ...
Paramétrage de crudini (0.9.4-1) ...
Paramétrage de libv4l-0:arm64 (1.22.1-5+b2) ...
Paramétrage de libevent-dev (2.1.12-stable-8) ...
Paramétrage de v4l-utils (1.22.1-5+b2) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.36-9+deb12u6) ...
Traitement des actions différées (« triggers ») pour man-db (2.11.2-2) ...
Install dependencies ... [OK]
Reading configuration ...
No user configuration file found ...
Using default configuration ... [OK]
Creating file structure ...
Directory /home/mks/printer_data/config already exists ...
Directory /home/mks/printer_data/logs already exists ...
Directory /home/mks/printer_data/systemd already exists ...
Creating file structure ... [OK]
Link crowsnest to /usr/local/bin ... [OK]
Install service file ... [OK]
Install environment file ... [OK]
Install logrotate configuration ... [OK]
Install crowsnest.conf ... [OK]
Enable crowsnest.service ... [OK]
Add User mks to group 'video' ... [SKIPPED]
        ==> User mks is already in group 'video'
Build dependend Stream Apps ...
Cloning ustreamer repository ...
Clonage dans 'bin/ustreamer'...
remote: Enumerating objects: 201, done.
remote: Counting objects: 100% (201/201), done.
remote: Compressing objects: 100% (142/142), done.
remote: Total 201 (delta 69), reused 119 (delta 36), pack-reused 0
Réception d'objets: 100% (201/201), 178.44 Kio | 2.44 Mio/s, fait.
Résolution des deltas: 100% (69/69), fait.
Note : basculement sur '9ec59143dda445859e0ddb4027b44029ced5ac14'.

Vous êtes dans l'état « HEAD détachée ». Vous pouvez visiter, faire des modifications
expérimentales et les valider. Il vous suffit de faire un autre basculement pour
abandonner les commits que vous faites dans cet état sans impacter les autres branches

Si vous voulez créer une nouvelle branche pour conserver les commits que vous créez,
il vous suffit d'utiliser l'option -c de la commande switch comme ceci :

  git switch -c <nom-de-la-nouvelle-branche>

Ou annuler cette opération avec :

  git switch -

Désactivez ce conseil en renseignant la variable de configuration advice.detachedHead à false

Install of camera-streamer skipped, only supported on Raspberry SBC's! ...
Build 'ustreamer' using 2 Cores ...
make apps
make[1] : on entre dans le répertoire « /home/mks/crowsnest/bin/ustreamer »
make -C src
make[2] : on entre dans le répertoire « /home/mks/crowsnest/bin/ustreamer/src »
-- CC libs/base64.c
-- CC libs/capture.c
-- CC libs/fpsi.c
-- CC libs/frame.c
-- CC libs/frametext.c
-- CC libs/frametext_font.c
-- CC libs/logging.c
-- CC libs/memsink.c
-- CC libs/memsinksh.c
-- CC libs/options.c
-- CC libs/queue.c
-- CC libs/ring.c
-- CC libs/signal.c
-- CC libs/tc358743.c
-- CC libs/unjpeg.c
-- CC ustreamer/blank.c
-- CC ustreamer/data/favicon_ico.c
-- CC ustreamer/data/index_html.c
-- CC ustreamer/encoder.c
-- CC ustreamer/encoders/cpu/encoder.c
-- CC ustreamer/encoders/hw/encoder.c
-- CC ustreamer/http/bev.c
-- CC ustreamer/http/mime.c
-- CC ustreamer/http/path.c
-- CC ustreamer/http/server.c
-- CC ustreamer/http/static.c
-- CC ustreamer/http/unix.c
-- CC ustreamer/http/uri.c
-- CC ustreamer/m2m.c
-- CC ustreamer/main.c
-- CC ustreamer/options.c
-- CC ustreamer/stream.c
-- CC ustreamer/workers.c
-- CC dump/file.c
-- CC dump/main.c
== LD ustreamer.bin
== LD ustreamer-dump.bin
make[2] : on quitte le répertoire « /home/mks/crowsnest/bin/ustreamer/src »
for i in src/*.bin; do \
        test ! -x $i || ln -sf $i `basename $i .bin`; \
done
make[1] : on quitte le répertoire « /home/mks/crowsnest/bin/ustreamer »
Build 'ustreamer' ... [SUCCESS]
'/home/mks/crowsnest/bin/camera-streamer' does not exist! Build skipped ... [WARN]


Do you want to add 'update manager' entry to your moonraker.conf? [Y/n]: Y
Trying to add 'update manager' entry to moonraker.conf ...
Adding Crowsnest Update Manager entry to moonraker.conf ...  [OK]

Installation successful.

        Reboot your machine for the changes to take effect!

Reboot NOW? [y/N]: N

Not to reboot may cause issues!
Reboot as soon as possible!

Goodbye ...
``` 

</details>

> [!NOTE]
>
> Un redémarrage du système sera nécessaire pour prendre en compte cette installation de Crowsnest

### Gcode Shell Command

Permet d'étendre les fonctionnalités des macros G-code en utilisant des scripts shell

<details>

<summary>Gcode shell command</summary>

``` 
/=======================================================\
|     ~~~~~~~~~~~~~~~~~ [ KIAUH ] ~~~~~~~~~~~~~~~~~     |
|        Klipper Installation And Update Helper         |
|     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~     |
\=======================================================/
/=======================================================\
| You are about to install the 'G-Code Shell Command'   |
| extension. Please make sure to read the instructions  |
| before you continue and remember that potential risks |
| can be involved after installing this extension!      |
|                                                       |
| You accept that you are doing this on your own risk!  |
\=======================================================/
###### Do you want to continue? (Y/n):
   [➔] Yes

###### Installing gcode shell command extension ...

###### Stop klipper.service ...
[✓ OK] Stop klipper.service successfull!

###### Copy 'gcode_shell_command.py' to '/home/mks/klipper/klippy/extras' ...
[✓ OK] Done!

###### Create an example shell command? (Y/n):
   [➔] Yes

###### Timestamp: 240501-1727

###### Create backup of /home/mks/printer_data/config ...
[✓ OK] Backup created in:
/home/mks/kiauh-backups/configs/240501-1727/printer_data

###### Copy shell_command.cfg to /home/mks/printer_data/config ...
[✓ OK] /home/mks/printer_data/config/shell_command.cfg created!

###### Restart klipper.service ...
[✓ OK] Restart klipper.service successfull!

#=======================================================#
 Shell command extension installed!
#=======================================================# 
```

</details>

> [!NOTE]
> 
> Le menu Update de KIAUH affiche les versions actuellement installées et les dernières en date sur les dépôts respectifs. Si besoin, on peut réaliser les mises à jour.
 
![KIAUH Update Menu](../Images/kiauh-update-menu.jpg)

A la fin de ces installations, l'écosystème Klipper est prêt. L'eMMC de 8Go est occupée à plus de 50%, il reste encore plus de 3Go disponibles (***avec le système originel, on disposait de moins de 1Go***).

> [!IMPORTANT]
>
> **Avant** le système occupe un peu moins de 30% :
> 
> ```
> mks@mkspi:~$ df -h
> Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
> udev               379M       0  379M   0% /dev
> tmpfs               92M    2,0M   90M   3% /run
> /dev/mmcblk1p2     6,7G    1,9G  4,8G  29% /
> tmpfs              456M       0  456M   0% /dev/shm
> tmpfs              5,0M       0  5,0M   0% /run/lock
> tmpfs              456M       0  456M   0% /tmp
> /dev/mmcblk1p1     256M     88M  169M  35% /boot
> /dev/zram1          47M    740K   43M   2% /var/log
> tmpfs               92M       0   92M   0% /run/user/1000
> ```
>
> **Après** :
> 
> ```
> mks@mkspi:~$ df -h
> Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
> udev               379M       0  379M   0% /dev
> tmpfs               92M    2,0M   90M   3% /run
> /dev/mmcblk1p2     6,7G    3,6G  3,1G  55% /
> tmpfs              456M       0  456M   0% /dev/shm
> tmpfs              5,0M       0  5,0M   0% /run/lock
> tmpfs              456M       0  456M   0% /tmp
> /dev/mmcblk1p1     256M     88M  169M  35% /boot
> /dev/zram1          47M    740K   43M   2% /var/log
> tmpfs               92M       0   92M   0% /run/user/1000
> ```

L'accès à l'imprimante via Fluidd et/ou Mainsail affiche des messages d'erreurs.

*Mainsail*

![Erreurs Mainsail](../Images/mainsail-error-no-printercfg.jpg)

*Fluidd*

![Erreurs Fluidd](../Images/fluidd-error-no-printercfg.jpg)

Le fichier de configuration, `printer.cfg` installé par Klipper est minimaliste, une fois remplacé par celui de Qiditech, les erreurs sont plus précises:

![Erreurs Mainsail](../Images/mainsail-error-versions-firmwares.jpg)

![Erreurs Mainsail](../Images/fluidd-error-klipper-versions-firmwares.jpg)

> [!TIP]
> Mainsail (et Fluidd) possède une bonne documentation.
> [Ce lien (Mainsail)](https://docs.mainsail.xyz/faq/klipper_errors/command-format-mismatch) explique la raison de ce message d'erreur «**MCU protocol error**»:
>
> ***De temps en temps, des changements sont réalisés dans la communication Klipper entre l'hôte et le MCU. Cela nécessite de mettre à jour tous vos MCU pour corriger l'erreur de communication.***

**Le firmware Klipper n'est à jour sur aucun des MCUs utilisés**. Le dépôt Klipper est en version 0.12.x alors que les firmwares Klipper flashés sur les MCUs sont en version 0.10.x (version utilisée par Qidi Tech).

Il reste à installer le firmware Klipper sur les MCU (**M**icro **C**ontroler **U**nit) de l'imprimante :
- carte contrôleur (STM32F402)
- la carte MKS-THR de la tête (RP2040)
- le Linux MCU (nommé fréquemment «rpi»)

 C'est l'objet de cette dernière étape décrite [ici](./firmware-mcus.md).

😃
