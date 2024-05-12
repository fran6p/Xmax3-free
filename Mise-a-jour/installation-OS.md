# Installation d'un système plus à jour

> [!IMPORTANT]
> 
> Qidi Tech a choisi pour ses derniers modèles (Series 3 et Q1 Pro) d'utiliser le firmware Klipper.
>
> Lors des étapes de développement, leur choix a été de figer les versions des briques de l'écosystème Klipper.
>
> En restant sur des versions anciennes mais «identiques» pour tous leurs modèles, cela leur permet d'apporter une maintenance plus aisée plus difficile à assurer si chaque utilisateur installait des versions des composants essentiels (Klipper, Moonraker) différentes.
> 
> C'est un point de vue commercial honorable, d'autant plus qu'ầ ce jour, leur SAV est irréprochable.
>
> Honnêtement depuis maintenant plus de neuf mois que j'utilise ma X-Max 3 (*reçue en août 2023*), je n'ai pas rencontré de problèmes majeurs à cause de ces choix logiciels originaux.

## Pourquoi ?

La distribution Linux installée d'origine est une Armbian en version Buster, version passée en fin de vie depuis quelques années déjà.

Armbian dont le développement est bénévole, a supprimé depuis quelques mois les dépôts Buster de ses serveurs (voir [ici](https://fi.mirror.armbian.de/apt/dists/)). 

En conséquences, la mise à jour du système remonte des erreurs au sujet de ces dépôts dorénavant inaccessibles :

```
Ign:5 http://fi.mirror.armbian.de/apt buster InRelease
Err:6 http://fi.mirror.armbian.de/apt buster Release
  404  Not Found [IP: 65.21.120.247 80]
```

**Les seules mises à jour disponibles du système ne concernent plus que celles des dépôts Debian** (Armbian est basée sur cette distribution).

<details>

   <summary>Solution provisoire</summary>

```
Commenter la ligne faisant référence au dépôt Buster d'Armbian dans le fichier /etc/apt/sources.list.d/armbian.list

#deb http://apt.armbian.com buster main buster-utils buster-desktop

```
   
</details>

Avec Armbian/Debian Buster, **Python cohabite en deux versions** (v2 et v3), la v2, passée en fin de vie, il y a plusieurs années désormais, n'est plus supportée. La v3 reste bloquée en version 3.7.

Le Klipper (klippy) proposé par Qidi Tech a été installé avec l'environnement Python2. Klipper préconise actuellement de construire cet environnement virtuel (klippy) en utilisant Python en version 3.

> [!CAUTION]
> 
> **En suivant ces instructions, vous perdrez la garantie de Qidi Tech**

Au moins deux éléments matériels ne seront plus directement utilisables :
- l'actuel écran tactile,
- l'accès réseau Wifi avec la clé Wifi Tenda.

<details>
  <summary>On peut y remédier</summary>
  
> [CONSEIL]
> 
> Moyennant achats supplémentaires, on peut compenser la perte de l'écran originel et du Wifi :
> 
> - Klipperscreen avec un autre écran et un SBC (Small Board Computer) pour piloter l'imprimante
> - Une clé USB Wifi gérée nativement par Armbian ([une bonne liste](https://teamgloomy.github.io/armbian_wifi.html)). Il faudra connaitre précisément le fabricant et la puce (chipset) utilisés.

</details>

> [!WARNING]
> 
> Les manipulations décrites s'adressent à un utilisateur ayant un peu d'expérience avec Linux et sa ligne de commandes ou un débutant bien motivé acceptant de franchir la courbe d'apprentissage afin de progresser.
>
> En gros, je n'assume aucune responsabilité en cas de mauvaises manipulations. Ce qui a fonctionné pour moi peut très bien s'avérer un cauchemar pour d'autres… comme disent les anglo-saxons ***YMMV***.
> 
> J'insiste à nouveau:
> 
> **Les manipulations décrites sont faites à vos risques et périls. Vous ne devez pas contacter l'assistance QIDI en cas de problème. En effectuant ces modifications, vous perdrez votre garantie à cet égard.**
>
> Qidi cependant fournit une image de "récupération" qui permettra de restaurer le système dans l'état où QIDI livre ses imprimantes. Vous n'aurez alors qu'à "rétrograder" (flasher à nouveau le firmware Klipper) sur la tête «THR», le MCU de la carte contrôleur et celui du Linux MCU.
> Vous serez pratiquement revenu au point de départ et vous devriez pouvoir retrouver votre garantie 😃


## Prérequis

Matériel:

- Lecteur eMMC (si achat de l'eMMC de 32Gio de Qidi, un adaptateur μSD est inclus). Je préfère [cet adaptateur là](https://www.aliexpress.com/item/1005005614719377.html).

Logiciels (*à télécharger si nécessaire* (⏬)) :
- ⏬ [Rufus](https://rufus.ie/fr/) ou ⏬ [BalenaEtcher](https://etcher.balena.io/) ou encore ⏬ [Raspberry Imager](https://www.raspberrypi.com/software/) (au choix)
- ⏬ [image récente du système Armbian](https://github.com/redrathnure/armbian-mkspi/releases/tag/mkspi%2F0.3.4-24.2.0-trunk) (*au moment de la rédaction de  cette documentation (20240402), j'utilise cette version (Bookworm (24.2.0), noyau (6.6.17)* )
  - Merci à [@redrathnure](https://github.com/redrathnure/armbian-mkspi) qui a réalisé l'image du système Armbian récent
- accès SSH (⏬ [Putty](https://putty.org/), ⏬ [MobaXterm](https://mobaxterm.mobatek.net/), …)
- transfert de fichier (⏬ [WinSCP](https://winscp.net/eng/index.php) )
- archiveur de fichiers ⏬ [7zip](https://7-zip.org/) 

## Préalable

> [!IMPORTANT]
> 
> **Le système d'exploitation sera complètement remplacé par un plus récent**
   
>[!TIP]
>
> Avant toute chose, il est préférable d'avoir réalisé une sauvegarde de la totalité des dossiers:
> - ~/klipper_config (contient les fichiers de configuration)
> - ~/gcode_files (contient les G-codes).

Une fois ces précautions prises :
- éteindre l'imprimante et débrancher le câble d'alimentation
- accéder à l'arrière pour démonter la plaque donnant accès à la partie électronique
- dévisser les deux vis maintenant l'eMMC sur la carte et extraire celle-ci délicatement en veuillant à l'extraire «verticalement» ( *se mettre à la terre avant de faire  ces manipulations est une bonne pratique pour toute manipulation de carte électronique* 😏 ). Qidi met à disposition [cette vidéo](https://drive.google.com/drive/folders/1EPYKbYz4ecUIf17z5wtP-jDAOPeDkXJP) montrant la procédure.


## Installation Armbian Bookworm

### Sur le «PC»

- connecter l'eMMC à l'aide de son adaptateur sur le PC de travail
- démarrer Rufus pour flasher l'image Armbian précédemment téléchargée (***au moment de la rédaction : Armbian-unofficial_24.2.0-trunk_Mkspi_bookworm_current_6.6.17.img.xz***) puis décompressée via 7zip pour obtenir le fichier d'extension .img (Armbian-unofficial_24.2.0-trunk_Mkspi_bookworm_current_6.6.17.img)

![](../Images/rufus-1.jpg)

  * sélectionner le périphérique contenant l'eMMC (1)
  * indiquer l'emplacement du ficher .img (2)
  * (3) permet de vérifier l'intégrité du fichier téléchargé (le fichier .sha du dépôt Github contient l'empreinte)
  * lancer le processus de flashage (4)

Des fenêtres d'alerte peuvent s'afficher, cliquer sur OK pour valider et poursuivre le flashage

![](../Images/rufus-3.jpg)
![](../Images/rufus-2.jpg)

La procédure dure un certain temps (moins de dix minutes), la progression de la tâche s'affiche au fur et à mesure en bas de la fenêtre dans la zone STATUT

![](../Images/rufus-4.jpg)

finalement, arrivée à 100%, le statut passe au vert et indique que c'est prêt:

![](../Images/rufus-5.jpg)

retirer alors le lecteur contenant l'eMMC via la procédure standard pour l'éjecter proprement du PC

Le nouveau système d'exploitation (Armbian, version Bookworm) est installé sur la carte eMMC.

### Sur l'imprimante

- réinsérer l'eMMC sur la carte contrôleur de l'imprimante (voir la vidéo citée plus haut)
- brancher le câble d'alimentation **et le câble Ethernet** (rappel: la clé Wifi Tenda n'est plus fonctionnelle)
- allumer l'imprimante (le système démarre, une adresse IP est attribuée (Box internet, routeur)
  - le firmware indépendant de l'écran ne pouvant plus communiquer avec le système (absence des «logiciels» le permettant), l'écran affiche ceci :
  - ![Écran en erreur!!!](../Images/BSOD-screen.jpg)
- récupérer l'adresse IP par le moyen qui convient (Box internet, routeur, logiciel type [AngryIP](https://angryip.org/)
- démarrer le logiciel SSH (Putty, SSH natif, Mobaxterm, …) et accéder à l'imprimante via son adresse IP
- le premier accès se fait en tant qu'utilisateur "**root**", mot de passe "**1234**". Cette première connexion lance le setup initial du systéme Armbian (changement du mot de passe "root", choix de la zone horaire, choix du shell entre autre)
- ensuite il est demandé de créer un nouvel utilisateur ( par exemple: "**mks**", mot de passe "***makerbase***" ), confirmer par ENTRÉE. Xz nouvel utilisateur possède des droits équivalents à «root» (sudo)
- chez moi, la zone horaire (timezone) ayant été détectée (Europe/Paris), l'installateur propose de générer les locales. Plusieurs choix étant possible, je sélectionne le 4 (fr_FR.UTF-8)
- à l'aide du logiciel SSH, ouvrir une nouvelle session en tant que le nouvel utilisateur créé ( "**mks**" )
- mettre à jour le système :

```sh
sudo apt update
sudo apt upgrade
```
ou en une seule ligne
```sh
sudo apt update && sudo apt upgrade
```
Confirmer la mise à jour (manuellement) ou ajouter le paramètre "-y" à la suite de "upgrade" 
```sh
sudo apt upgrade -y
```

<details>
  <summary>Apercu des étapes ci-dessus</summary>

  ```
Welcome to Armbian-unofficial!

Documentation: https://docs.armbian.com/ | Community support: https://community.armbian.com/

IP address: 192.168.1.126

Create root password: *********
Repeat root password: *********

Choose default system command shell:

1) bash
2) zsh
1

Shell: BASH

Creating a new user account. Press <Ctrl-C> to abort

Please provide a username (eg. your first name): mks
Create user (mks) password: *********
Repeat user (mks) password: *********

Please provide your real name: Mks

Dear Mks, your account mks has been created and is sudo enabled.
Please use this account for your daily work from now on.

Detected timezone: Europe/Paris

Set user language based on your location? [Y/n]
Y

At your location, more locales are possible:

1) br_FR.UTF-8              5) ia_FR
2) ca_FR.UTF-8              6) oc_FR.UTF-8
3) eu_FR.UTF-8              7) Skip generating locales
4) fr_FR.UTF-8
Please enter your choice:4

Generating locales: fr_FR.UTF-8

root@mkspi:~# exit
…
=> reconnexion en utilisateur «mks»
login as: mks
mks@192.168.1.126's password:
           _              _
 _ __ ___ | | _____ _ __ (_)
| '_ ` _ \| |/ / __| '_ \| |
| | | | | |   <\__ \ |_) | |
|_| |_| |_|_|\_\___/ .__/|_|
                   |_|
Welcome to Armbian-unofficial 24.2.0-trunk Bookworm with Linux 6.6.17-current-rockchip64

No end-user support: built from trunk

System load:   3%               Up time:       3:21
Memory usage:  20% of 911M      IP:            192.168.1.126
CPU temp:      40°C             Usage of /:    56% of 6.7G

[ Kernel and firmware upgrades disabled: armbian-config ]
Last check: 2024-05-12 11:10

[ General system configuration (beta): armbian-config ]

Last login: Sun May 12 13:38:15 2024 from 192.168.1.101
mks@mkspi:~$
mks@mkspi:~$ sudo apt update
Réception de :1 http://security.debian.org bookworm-security InRelease [48,0 kB]
Atteint :2 http://deb.debian.org/debian bookworm InRelease
Réception de :3 http://deb.debian.org/debian bookworm-updates InRelease [55,4 kB]
Réception de :5 http://deb.debian.org/debian bookworm-backports InRelease [56,5 kB]
Réception de :4 http://imola.armbian.com/apt bookworm InRelease [53,3 kB]
Réception de :6 http://security.debian.org bookworm-security/main arm64 Packages [152 kB]
Réception de :7 http://deb.debian.org/debian bookworm-updates/main arm64 Packages.diff/Index [10,6 kB]
Réception de :8 http://deb.debian.org/debian bookworm-updates/main arm64 Contents (deb).diff/Index [8361 B]
Réception de :9 http://deb.debian.org/debian bookworm-updates/main arm64 Packages T-2024-04-23-2036.10-F-2024-04-23-2036.10.pdiff [1597 B]
Réception de :9 http://deb.debian.org/debian bookworm-updates/main arm64 Packages T-2024-04-23-2036.10-F-2024-04-23-2036.10.pdiff [1597 B]
Réception de :10 http://deb.debian.org/debian bookworm-updates/main arm64 Contents (deb) T-2024-04-23-2036.10-F-2024-04-23-2036.10.pdiff [6870 B]
Réception de :10 http://deb.debian.org/debian bookworm-updates/main arm64 Contents (deb) T-2024-04-23-2036.10-F-2024-04-23-2036.10.pdiff [6870 B]
Réception de :11 http://deb.debian.org/debian bookworm-backports/main arm64 Packages.diff/Index [63,3 kB]
Ign :11 http://deb.debian.org/debian bookworm-backports/main arm64 Packages.diff/Index
Réception de :12 http://deb.debian.org/debian bookworm-backports/main all Contents (deb).diff/Index [63,6 kB]
Réception de :13 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb).diff/Index [63,4 kB]
Réception de :14 http://deb.debian.org/debian bookworm-backports/contrib arm64 Packages.diff/Index [25,2 kB]
Réception de :15 http://deb.debian.org/debian bookworm-backports/contrib all Contents (deb).diff/Index [12,8 kB]
Réception de :16 http://deb.debian.org/debian bookworm-backports/contrib arm64 Contents (deb).diff/Index [7239 B]
Réception de :17 http://deb.debian.org/debian bookworm-backports/non-free arm64 Packages.diff/Index [4995 B]
Réception de :18 http://deb.debian.org/debian bookworm-backports/non-free all Contents (deb).diff/Index [3873 B]
Réception de :19 http://imola.armbian.com/apt bookworm/main all Packages [4650 B]
Réception de :20 http://imola.armbian.com/apt bookworm/main arm64 Packages [188 kB]
Réception de :21 http://deb.debian.org/debian bookworm-backports/main all Contents (deb) T-2024-05-01-0805.42-F-2024-02-19-2010.17.pdiff [539 kB]
Réception de :21 http://deb.debian.org/debian bookworm-backports/main all Contents (deb) T-2024-05-01-0805.42-F-2024-02-19-2010.17.pdiff [539 kB]
Réception de :22 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb) T-2024-04-29-1406.02-F-2024-02-19-2010.17.pdiff [321 kB]
Réception de :23 http://imola.armbian.com/apt bookworm/main arm64 Contents (deb) [5030 kB]
Réception de :22 http://deb.debian.org/debian bookworm-backports/main arm64 Contents (deb) T-2024-04-29-1406.02-F-2024-02-19-2010.17.pdiff [321 kB]
Réception de :24 http://deb.debian.org/debian bookworm-backports/contrib arm64 Packages T-2024-04-16-1405.22-F-2024-03-07-1416.30.pdiff [1825 B]
Réception de :24 http://deb.debian.org/debian bookworm-backports/contrib arm64 Packages T-2024-04-16-1405.22-F-2024-03-07-1416.30.pdiff [1825 B]
Réception de :25 http://deb.debian.org/debian bookworm-backports/contrib all Contents (deb) T-2024-03-07-1416.30-F-2024-03-07-1416.30.pdiff [6356 B]
Réception de :25 http://deb.debian.org/debian bookworm-backports/contrib all Contents (deb) T-2024-03-07-1416.30-F-2024-03-07-1416.30.pdiff [6356 B]
Réception de :26 http://deb.debian.org/debian bookworm-backports/contrib arm64 Contents (deb) T-2024-03-07-1416.30-F-2024-03-07-1416.30.pdiff [574 B]
Réception de :26 http://deb.debian.org/debian bookworm-backports/contrib arm64 Contents (deb) T-2024-03-07-1416.30-F-2024-03-07-1416.30.pdiff [574 B]
Réception de :27 http://deb.debian.org/debian bookworm-backports/non-free arm64 Packages T-2024-04-22-2005.41-F-2024-02-19-2010.17.pdiff [257 B]
Réception de :27 http://deb.debian.org/debian bookworm-backports/non-free arm64 Packages T-2024-04-22-2005.41-F-2024-02-19-2010.17.pdiff [257 B]
Réception de :28 http://deb.debian.org/debian bookworm-backports/non-free all Contents (deb) T-2024-02-19-2010.17-F-2024-02-19-2010.17.pdiff [3326 B]
Réception de :28 http://deb.debian.org/debian bookworm-backports/non-free all Contents (deb) T-2024-02-19-2010.17-F-2024-02-19-2010.17.pdiff [3326 B]
Réception de :29 http://deb.debian.org/debian bookworm-backports/main arm64 Packages [189 kB]
Réception de :30 http://imola.armbian.com/apt bookworm/main all Contents (deb) [31,7 kB]
Réception de :31 http://imola.armbian.com/apt bookworm/bookworm-utils all Packages [2357 B]
Réception de :32 http://imola.armbian.com/apt bookworm/bookworm-utils arm64 Packages [15,9 kB]
Réception de :33 http://imola.armbian.com/apt bookworm/bookworm-utils all Contents (deb) [12,1 kB]
Réception de :34 http://imola.armbian.com/apt bookworm/bookworm-utils arm64 Contents (deb) [21,2 kB]
Réception de :35 http://imola.armbian.com/apt bookworm/bookworm-desktop all Packages [933 B]
Réception de :36 http://imola.armbian.com/apt bookworm/bookworm-desktop arm64 Packages [1965 B]
Réception de :37 http://imola.armbian.com/apt bookworm/bookworm-desktop arm64 Contents (deb) [7694 B]
Réception de :38 http://imola.armbian.com/apt bookworm/bookworm-desktop all Contents (deb) [385 B]
7015 ko réceptionnés en 12s (581 ko/s)
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
27 paquets peuvent être mis à jour. Exécutez « apt list --upgradable » pour les voir.
mks@mkspi:~$ sudo apt list --upgradable
En train de lister... Fait
armbian-config/bookworm,bookworm 24.2.1 all [pouvant être mis à jour depuis : 24.2.0-trunk]
armbian-firmware/bookworm,bookworm 24.2.1 all [pouvant être mis à jour depuis : 24.2.0-trunk]
armbian-plymouth-theme/bookworm,bookworm 24.2.1 all [pouvant être mis à jour depuis : 24.2.0-trunk]
armbian-zsh/bookworm,bookworm 24.2.1 all [pouvant être mis à jour depuis : 24.2.0-trunk]
base-files/bookworm 24.2.1-12.4+deb12u5-bookworm arm64 [pouvant être mis à jour depuis : 24.2.0-trunk-12.4+deb12u5-bookworm]
bsdextrautils/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
bsdutils/stable-security 1:2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 1:2.38.1-5+b1]
fdisk/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
libblkid1/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
libc-bin/stable-security 2.36-9+deb12u6 arm64 [pouvant être mis à jour depuis : 2.36-9+deb12u4]
libc-dev-bin/stable-security 2.36-9+deb12u6 arm64 [pouvant être mis à jour depuis : 2.36-9+deb12u4]
libc-l10n/stable-security 2.36-9+deb12u6 all [pouvant être mis à jour depuis : 2.36-9+deb12u4]
libc6-dev/stable-security 2.36-9+deb12u6 arm64 [pouvant être mis à jour depuis : 2.36-9+deb12u4]
libc6/stable-security 2.36-9+deb12u6 arm64 [pouvant être mis à jour depuis : 2.36-9+deb12u4]
libfdisk1/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
libmount1/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
libsmartcols1/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
libuuid1/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
linux-dtb-current-rockchip64/bookworm 24.2.1 arm64 [pouvant être mis à jour depuis : 24.2.0-trunk]
linux-headers-current-rockchip64/bookworm 24.2.1 arm64 [pouvant être mis à jour depuis : 24.2.0-trunk]
linux-image-current-rockchip64/bookworm 24.2.1 arm64 [pouvant être mis à jour depuis : 24.2.0-trunk]
linux-libc-dev/stable-security 6.1.85-1 arm64 [pouvant être mis à jour depuis : 6.1.76-1]
locales/stable-security 2.36-9+deb12u6 all [pouvant être mis à jour depuis : 2.36-9+deb12u4]
mount/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
rfkill/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
util-linux-extra/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
util-linux/stable-security 2.38.1-5+deb12u1 arm64 [pouvant être mis à jour depuis : 2.38.1-5+b1]
mks@mkspi:~$ sudo apt upgrade
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances... Fait
Lecture des informations d'état... Fait
Calcul de la mise à jour... Fait
Les paquets suivants ont été conservés :
  armbian-config armbian-firmware armbian-plymouth-theme armbian-zsh
  base-files linux-dtb-current-rockchip64 linux-headers-current-rockchip64
  linux-image-current-rockchip64
Les paquets suivants seront mis à jour :
  bsdextrautils bsdutils fdisk libblkid1 libc-bin libc-dev-bin libc-l10n libc6
  libc6-dev libfdisk1 libmount1 libsmartcols1 libuuid1 linux-libc-dev locales
  mount rfkill util-linux util-linux-extra
19 mis à jour, 0 nouvellement installés, 0 à enlever et 8 non mis à jour.
Il est nécessaire de prendre 13,2 Mo dans les archives.
Après cette opération, 26,6 ko d'espace disque supplémentaires seront utilisés.
Souhaitez-vous continuer ? [O/n]
Réception de :1 http://security.debian.org bookworm-security/main arm64 bsdutils arm64 1:2.38.1-5+deb12u1 [94,0 kB]
Réception de :2 http://security.debian.org bookworm-security/main arm64 libc6-dev arm64 2.36-9+deb12u6 [1430 kB]
Réception de :3 http://security.debian.org bookworm-security/main arm64 libc-dev-bin arm64 2.36-9+deb12u6 [44,7 kB]
Réception de :4 http://security.debian.org bookworm-security/main arm64 linux-libc-dev arm64 6.1.85-1 [1944 kB]
Réception de :5 http://security.debian.org bookworm-security/main arm64 libc6 arm64 2.36-9+deb12u6 [2321 kB]
Réception de :6 http://security.debian.org bookworm-security/main arm64 libsmartcols1 arm64 2.38.1-5+deb12u1 [104 kB]
Réception de :7 http://security.debian.org bookworm-security/main arm64 util-linux-extra arm64 2.38.1-5+deb12u1 [112 kB]
Réception de :8 http://security.debian.org bookworm-security/main arm64 util-linux arm64 2.38.1-5+deb12u1 [1144 kB]
Réception de :9 http://security.debian.org bookworm-security/main arm64 libc-bin arm64 2.36-9+deb12u6 [531 kB]
Réception de :10 http://security.debian.org bookworm-security/main arm64 mount arm64 2.38.1-5+deb12u1 [134 kB]
Réception de :11 http://security.debian.org bookworm-security/main arm64 libblkid1 arm64 2.38.1-5+deb12u1 [143 kB]
Réception de :12 http://security.debian.org bookworm-security/main arm64 libmount1 arm64 2.38.1-5+deb12u1 [159 kB]
Réception de :13 http://security.debian.org bookworm-security/main arm64 libuuid1 arm64 2.38.1-5+deb12u1 [28,3 kB]
Réception de :14 http://security.debian.org bookworm-security/main arm64 libfdisk1 arm64 2.38.1-5+deb12u1 [182 kB]
Réception de :15 http://security.debian.org bookworm-security/main arm64 fdisk arm64 2.38.1-5+deb12u1 [139 kB]
Réception de :16 http://security.debian.org bookworm-security/main arm64 libc-l10n all 2.36-9+deb12u6 [675 kB]
Réception de :17 http://security.debian.org bookworm-security/main arm64 locales all 2.36-9+deb12u6 [3902 kB]
Réception de :18 http://security.debian.org bookworm-security/main arm64 bsdextrautils arm64 2.38.1-5+deb12u1 [86,9 kB]
Réception de :19 http://security.debian.org bookworm-security/main arm64 rfkill arm64 2.38.1-5+deb12u1 [37,1 kB]
13,2 Mo réceptionnés en 2s (8046 ko/s)
Preconfiguring packages ...
(Lecture de la base de données... 68171 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../bsdutils_1%3a2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de bsdutils (1:2.38.1-5+deb12u1) sur (1:2.38.1-5+b1) ...
Paramétrage de bsdutils (1:2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68170 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libc6-dev_2.36-9+deb12u6_arm64.deb ...
Dépaquetage de libc6-dev:arm64 (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Préparation du dépaquetage de .../libc-dev-bin_2.36-9+deb12u6_arm64.deb ...
Dépaquetage de libc-dev-bin (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Préparation du dépaquetage de .../linux-libc-dev_6.1.85-1_arm64.deb ...
Dépaquetage de linux-libc-dev:arm64 (6.1.85-1) sur (6.1.76-1) ...
Préparation du dépaquetage de .../libc6_2.36-9+deb12u6_arm64.deb ...
Dépaquetage de libc6:arm64 (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Paramétrage de libc6:arm64 (2.36-9+deb12u6) ...
(Lecture de la base de données... 68170 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libsmartcols1_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de libsmartcols1:arm64 (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de libsmartcols1:arm64 (2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68169 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../util-linux-extra_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de util-linux-extra (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de util-linux-extra (2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68168 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../util-linux_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de util-linux (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de util-linux (2.38.1-5+deb12u1) ...
fstrim.service is a disabled or a static unit not running, not starting it.
(Lecture de la base de données... 68167 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libc-bin_2.36-9+deb12u6_arm64.deb ...
Dépaquetage de libc-bin (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Paramétrage de libc-bin (2.36-9+deb12u6) ...
(Lecture de la base de données... 68167 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../mount_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de mount (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Préparation du dépaquetage de .../libblkid1_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de libblkid1:arm64 (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de libblkid1:arm64 (2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68165 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libmount1_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de libmount1:arm64 (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de libmount1:arm64 (2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68164 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../libuuid1_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de libuuid1:arm64 (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de libuuid1:arm64 (2.38.1-5+deb12u1) ...
(Lecture de la base de données... 68163 fichiers et répertoires déjà installés.)
Préparation du dépaquetage de .../0-libfdisk1_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de libfdisk1:arm64 (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Préparation du dépaquetage de .../1-fdisk_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de fdisk (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Préparation du dépaquetage de .../2-libc-l10n_2.36-9+deb12u6_all.deb ...
Dépaquetage de libc-l10n (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Préparation du dépaquetage de .../3-locales_2.36-9+deb12u6_all.deb ...
Dépaquetage de locales (2.36-9+deb12u6) sur (2.36-9+deb12u4) ...
Préparation du dépaquetage de .../4-bsdextrautils_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de bsdextrautils (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Préparation du dépaquetage de .../5-rfkill_2.38.1-5+deb12u1_arm64.deb ...
Dépaquetage de rfkill (2.38.1-5+deb12u1) sur (2.38.1-5+b1) ...
Paramétrage de libc-l10n (2.36-9+deb12u6) ...
Paramétrage de bsdextrautils (2.38.1-5+deb12u1) ...
Paramétrage de linux-libc-dev:arm64 (6.1.85-1) ...
Paramétrage de locales (2.36-9+deb12u6) ...
Generating locales (this might take a while)...
  en_US.UTF-8... done
  fr_FR.UTF-8... done
Generation complete.
Paramétrage de rfkill (2.38.1-5+deb12u1) ...
Paramétrage de libfdisk1:arm64 (2.38.1-5+deb12u1) ...
Paramétrage de mount (2.38.1-5+deb12u1) ...
Paramétrage de libc-dev-bin (2.36-9+deb12u6) ...
Paramétrage de fdisk (2.38.1-5+deb12u1) ...
Paramétrage de libc6-dev:arm64 (2.36-9+deb12u6) ...
Traitement des actions différées (« triggers ») pour man-db (2.11.2-2) ...
Traitement des actions différées (« triggers ») pour libc-bin (2.36-9+deb12u6) ...
mks@mkspi:~$
```

</details>

Le système de base est installé sur l'eMMC, il occupe moins de 2Go sur la capacité totale de 8Go

```
mks@mkspi:~$ df -h
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               379M       0  379M   0% /dev
tmpfs               92M    2,0M   90M   3% /run
/dev/mmcblk1p2     6,7G    1,9G  4,8G  29% /
tmpfs              456M       0  456M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
tmpfs              456M       0  456M   0% /tmp
/dev/mmcblk1p1     256M     88M  169M  35% /boot
/dev/zram1          47M    740K   43M   2% /var/log
tmpfs               92M       0   92M   0% /run/user/1000
```

### «Améliorer» l'OS

> [!TIP]
> Tel quel le système d'exploitation est pleinement fonctionnel
> 
> Certaines fonctionnalités de l'ancien OS de Makerbase ne sont plus disponibles, par exemple:
> - plus d'automontage d'une clé USB
> - le nom des interfaces réseau adopte celui «imposé» par systemd (je préfére l'ancien (ethX, wlanX)
> - l'utilisateur **mks** ne peut accéder aux GPIO, le groupe gpio n'existe pas, on ne peut accéder aux GPIO que via sudo
> - pas de synchronisation régulière du système de fichiers (/bin/sync)
> - quelques paquets doivent encore être installés (python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev python3-serial)
> - …

<details>
  
<summary>Y accéder</summary>

Ce ne sont que des améliorations cosmétiques 😏, pour les ajouter [voir ici](./addons-os.md)

</details>

[La documentation suivante](./installation-ecosysteme-klipper.md) permet de poursuivre l'installation avec l'écosystème Klipper

😃
