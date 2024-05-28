# FIRMWARE KLIPPER

## SOURCES

Les sources m'ayant servi à la rédaction des tutoriels :
- Mark Ewert, @melvenx, a réalisé [ce document PDF](https://github.com/melvenx/QIDI/blob/main/How%20to%20Update%20Klipper%20for%20the%20QIDI%20Tech%20XMAX3%20XPLUS3%20XSMART3%20Printers%20v2.pdf) relatant les étapes pour flasher les firmwares avec l'**OS originel de Qidi (Armbian Buster de Makerbase))**. Il est le premier, à ma connaissance, à avoir révélé le nom «spécifique» du fichier à utiliser pour flasher Klipper sur le micro-contrôleur STM32F401 ( **X_4.bin** ), étape primordiale pour réussir les flashages divers.
- Un site [OpenQIDI](https://openqidi.com) avait, dans un premier temps, mis à disposition une documentation (livre) supprimée depuis par l'utilisateur @phill1988 qui a remis à disposition cette documentation sur ce dépôt [FreeQidi](https://github.com/Phil1988/FreeQIDI) => **OS Armbian Bookworm**
- @leadustin a ensuite fourni une documentation plus étoffée d'abord en langue allemande puis [en anglais](https://github.com/leadustin/QIDI-up2date-english) => **OS Armbian Bookworm**
- un des développeurs chargé chez Qidi de l'écosystème Klipper ( @cchen616 ) décrit les étapes essentielles dans [cette issue Github](https://github.com/QIDITECH/QIDI_PLUS3/issues/27#issuecomment-2073932891) (à partir de l'**OS originel (Armbian buster))**.

## FIRMWARE GÉNÉRALITÉS

Pour chacun des contrôleurs (MCU):
- principal
- secondaire (la tête «MKS-THR»)
- Linux

les étapes à suivre sont quasi identiques:

1. se placer dans le répertoire Klipper du dossier personnel de l'utilisateur «mks»: `cd ~/klipper` ou `cd /home/mks/klipper` ou si on est déjà dans le dossier perso `cd klipper` ou `cd ./klipper`
2. configurer le firmware: `make menuconfig`
  3. dans la fenêtre de configuration, sélectionner les paramètres adéquats (dépend de chaque MCU)
  4. sauvegarder cette configuration
3. préparer la compilation: `make clean`
4. compiler: `make` (le microcontrôleur RK3328 possèdant plusieurs coeurs, utiliser une compilation parallèle à l'aide de `make -j4`)
5. à l'issue de la compilation, récupérer le firmware dans le dossier **out** (~/klipper/out/). Le nom du fichier compilé dépend des directives de compilation ( klipper.bin / klipper.uf2 / klipper.elf )
6. flasher ce firmware sur le MCU ( la méthode dépend du MCU à flasher )

Connaissant le principe, on va pouvoir entrer dans les détails.

## PRÉREQUIS

Quelques outils sont nécessaires:
- clé Allen (Hexagonale) 2.0 mm
- carte μSD, clé USB ou adaptateur USB/μSD
- logiciels:
  - pour l'accès distant (SSH)
  - pour le transfert de fichiers entre matériels en réseau
 
## PRÉPARATION MATÉRIELLE

> [!NOTE]
>
> Imprimante **éteinte**
> 

- accéder à l'arrière de l'imprimante pour retirer les vis du capot renfermant l'électronique à l'aide de la clé Allen de 2.0 mm
- retirer ce capot (le ventilateur de refroidissement de la carte y est fixé)
- toujours à l'aide de la clé Allen de 2.0mm, retirer les quatre vis de fixation du capot arrière de la tête pour pouvoir accéder à la carte fille «MKS-THR», sérigraphiée A-4

> [!NOTE]
>
> Mettre sous tension l'imprimante
>

- accéder via un client SSH à l'imprimante via son adresse IP
- se connecter en utilisateur **mks** (mot de passe par défaut si inchangé: ***makerbase***)
- se déplacer dans le dossier klipper

  `cd klipper`
- vérifier que l'on est dans le dossier correct

  `pwd`
  
# MCU PRINCIPAL

## Configurer le firmware Klipper

Connecté en ssh, lancer la suite de commandes:
```
cd ~/klipper
make clean
make menuconfig
```
Le menu de configuration du firmware apparait, choisir les options :
- [X] cocher «Enable extra low-level»
- [X] STMicroelectronic STM32 comme famille de contrôleur
<details>
<summary>contrôleur STM32</summary>
 
<p align="center">
<img src="/Images/klipper-menuconfig-choix-stm32.jpg">
</p>

</details>

- [X] Processor model (STM32F401) (le STM32F402 de la carte en est un 😏)
<details>
<summary>modèle STM32F401</summary>
 
<p align="center">
<img src="/Images/klipper-menuconfig-choix-stm32-modele.jpg">
</p>

</details>

- [X] Bootloader offset (32 Kio bootloader)
<details>
<summary>chargeur de démarrage</summary>
 
<p align="center">
<img src="/Images/klipper-menuconfig-choix-stm32-bootloader.jpg">
</p>

</details>

- [X] Communication interface (Serial (on USART1 PA10/PA9)

<details>
<summary>Communication</summary>

<p align="center">
<img src="/Images/klipper-menuconfig-choix-stm32-com.jpg">
</p>
 
</details>

Pour la communication on utilise le mode UART; le contrôleur RK3328 est câblé directement au STM32F402 [voir ce schéma](https://github.com/fran6p/Xmax3-free/blob/main/Images/Qidi-mks-skipr-pins.png)

<details>
<summary>RK3328<->STM32</summary>

<p align="center">
<img src="/Images/RK3328-STM32-CNX.jpg">
</p>
 
</details>

Au final, la configuration complète doit être comme ci-dessous :
<p align="center">
<img src="/Images/klipper-menuconfig-stm32f402.jpg">
</p>

- une fois ces options sélectionnées, presser <kbd>Q</kbd> pour sortir de ce menu, valider par <kbd>Y</kbd> pour sauvegarder la configuration

<details>
<summary>Quitter et valider</summary>
 
<p align="center">
<img src="/Images/make-menuconfig-save.jpg"
</p>

</details>

## Compiler

- compiler le firmware `make -j4` (utiliser une compilation parallèle à l'aide de plusieurs coeurs du contrôleur RK3328)
- attendre que le processus se termine
<details>
<summary>Fin de la compilation</summary>

<p align="center">
<img src="/Images/klipper-compil-stm32f401.jpg"
</p>

</details>  

Le firmware a été compilé dans le dossier ~/klipper/out et porte le nom **klipper.bin**

## Flasher le firmware

Le processus de flashage utilise un carte μSD (formatée FAT32 d'une capacité de moins de 32 Go) introduite dans le lecteur de carte μSD situé sur la carte X-4 (X-6 pour les machines plus récentes)
<details>
<summary>Motherboard</summary>
 
<p align="center">
<img src="/Images/xmax3-mb.jpg"
</p>

</details>

> [!IMPORTANT]
>
> Pour que le flashage réussisse, le fichier klipper.bin doit porter un nom précis : **X_4.bin** (x majuscule, souligné, chiffre 4).

Reste à récupérer ce fichier, le renommer puis le copier à la racine de la carte μSD

### Récupérer (télécharger) le firmware

Plusieurs méthodes permettent de le faire :

1. copier dans le répertoire de configuration de Klipper (~/printer_data/config)
```
cp ~/klipper/out/klipper.bin ~/printer_data/config/X_4.bin
```
Le récupérer en utilisant Fluidd (clic droit sur le fichier, télécharger)

2. mettre à disposition le firmware sur le réseau local via le serveur web Python
```
exec sh -c 'python3 -m http.server -d "/home/mks/klipper/out" 8888' &
```
Une fois le serveur lancé (en tâche de fond) sur le port 8888:
- via un navigateur, accéder à l'adresse http://adresse-ip-imprimante:8888
- récupérer le fichier klipper.bin
- le renommer en X_4.bin
- le copier sur la carte μSD 

3. Si Samba a été ajouté au système, on peut accéder à son répertoire personnel

ecran samba
4. Si l'automontage de périphériques de stockage a été installé sur le système, utiliser une clé USB adaptateur SD :
```
cp ~/klipper/out/klipper.bin ~/printer_data/gcodes/USB/X_4.bin
```   

### Procéder au flashage

- une fois le fichier X_4.bin recopié à la racine de la carte μSD:
  - introduire celle-ci dans le lecteur de carte
  - allumer l'imprimante (si elle était allumée, l'éteindre et patienter au moins 30 secondes, le temps que le supercondensateur se décharge, allumer alors) 
  - patienter (***très peu de temps***), le temps que le processus de flashage se termine
 
 
# MCU Linux (contrôleur de la carte X-4 / X-6, Rockchip RK3328)

[Source](https://www.klipper3d.org/fr/RPi_microcontroller.html#microcontroleur-rpi)

> [!NOTE]
>
> Les microcontrôleurs dédiés au contrôle des imprimantes 3D disposent d'un nombre limité et pré-configuré de broches exposées pour gérer les principales fonctions d'impression (thermistances, extrudeurs, pilotes moteurs, …). L'utilisation du contrôleur, ici le Rockchip RK3328 où Klipper est installé en tant que MCU secondaire donne la possibilité d'utiliser directement les GPIO et les bus (i2c, spi) du contrôleur à l'intérieur de klipper

## Firmware Klipper (installer script RC, configurer, flasher)

### Installer le script RC

Une étape préliminaire est nécessaire: pour utiliser l'hôte comme MCU secondaire, un daemon système (**klipper_mcu**) doit être installé. Son démarrage devra se faire avant celui de Klipper
```
cd ~/klipper/
sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/
sudo systemctl enable klipper-mcu.service
```

### Configurer Klipper

```
cd ~/klipper/
make menuconfig
```
Le menu de configuration du firmware apparait, choisir les options :
- [X] cocher «Enable extra low-level»
- [X] Linux process comme contrôleur
<details>
<summary>Linux process</summary>
 
<p align="center">
<img src="/Images/klipper-menuconfig-choix-linux.jpg">
<img src="/Images/klipper-menuconfig-linux-process.jpg">
</p>

</details>

- une fois ces options sélectionnées, presser <kbd>Q</kbd> pour sortir de ce menu, valider par <kbd>Y</kbd> pour sauvegarder la configuration

<details>
<summary>Quitter et valider</summary>
 
<p align="center">
<img src="/Images/make-menuconfig-save.jpg"
</p>

</details>
 
### Flasher

- compiler et installer le nouveau code du microcontrôleur

```
sudo systemctl stop klipper
make flash
sudo systemctl start klipper
```

Pour utiliser ce «MCU», il faut ajouter au fichier printer.cfg la section suivante :

```
[mcu host]
serial: /tmp/klipper_host_mcu
```

# MCU tête (carte A-4. contrôleur RP2040)

Ici, le firmware Klipper peut être installé de deux façons:
1. la première nécessitera **à chaque flashage / reflashage le démontage du capot arrière de la tête** pour accéder au bouton poussoir «BOOTSEL» permettant de passer le controleur RP2040 en mode émulation de stockage
2. la seconde ne demandera l'étape ci-dessus qu'**une seule fois** pour l'installation d'un chargeur de démarrage permettant ensuite le flashage du controleur RP2040 via USB. Elle nécessite par contre l'installation supplémentaire du logiciel [KATAPULT (ex CANBOOT)](https://github.com/Arksine/katapult) de @arksine

## Méthode 1

### Préparer le firmware

Connecté en ssh, lancer la suite de commandes:
```
cd ~/klipper
make clean
make menuconfig
```
Le menu de configuration du firmware apparait, choisir les options :
- [X] cocher «Enable extra low-level»
- [X] RP2040 comme contrôleur
<details>
<summary>contrôleur RP2040</summary>
 
<p align="center">
<img src="/Images/klipper-menuconfig-choix-rp2040.jpg">
</p>

</details>

- **Pas de chargeur de démarrage**
- USB comme interface de communication

<details>
<summary>Configuration complète</summary>

<p align="center">
<img src="/Images/klipper-menuconfig-rp2040.jpg">
</p>
 
</details>

- une fois ces options sélectionnées, presser <kbd>Q</kbd> pour sortir de ce menu, valider par <kbd>Y</kbd> pour sauvegarder la configuration

<details>
<summary>Quitter et valider</summary>
 
<p align="center">
<img src="/Images/make-menuconfig-save.jpg"
</p>

</details>

- compiler le firmware `make`. On peut profiter d'une compilation parallèle en utilisant plusieurs coeurs du contrôleur RK3328 avec un `make -j4`
- attendre que le processus se termine
<details>
<summary>Fin de la compilation</summary>

<p align="center">
<img src="/Images/klipper-compil-rp2040-uf2.jpg"
</p>

</details>  

Le firmware a été compilé dans le dossier ~/klipper/out et porte le nom **klipper.uf2**

### Flasher le firmware

**Pour flasher ce firmware, le contrôleur RP2040 doit passer en mode émulation du stockage (BOOTSEL mode)**.
- éteindre l'imprimante et patienter au moins 30 secondes le temps que le supercondensateur se décharge complètement.
- le capot arrière de la tête étant démonté:
  - presser et maintenir enfoncé le bouton au bas de la carte nommé **BOOT**
  - allumer l'imprimante
  - **ne relâcher la pression sur ce bouton qu'une fois l'imprimante complètement démarrée**. 

<p align="center">
<img src="/Images/toolhead.jpg">
</p>

- relâcher le bouton BOOT quand la lumière interne de l'imprimante s'allume ou une fois l'écran affichant un problème de démarrage (le système d'exploitation ne comporte plus les logiciels permettant la communication entre la carte => le firmware de l'écran considère qu'il y a un problème 😏)
- se (re)connecter en ssh en utilisateur ***mks***
- vérifier que le RP2040 est bien en mode émulation de stockage:
  - `lsblk` doit afficher un périphérique sda (partition sda1), et/ou
  - `lsusb` permet également de vérifier que le RP2040 est passé dans le «bon» mode (**ID 2a8a:0003 Raspberry Pi RP2 Boot**):

<p align="center">
<img src="/Images/lsblk-sda1-automount.jpg">
<img src="/Images/rp2040-lsusb-boot.jpg>
</p>

- Si aucun périphérique sda1 n'apparait à la suite de la commande `lsblk` ou que le périphérique USB n'est pas `ID 2a8a:0003 Raspberry Pi RP2 Boot`:
  - presser et maintenir enfoncé le bouton BOOT,
  - presser et relâcher le bouton RESET,
  - relâcher alors le bouton BOOT.
  - vérifier à nouveau avec un `lsblk` et/ou un `lsusb`

> [!TIP]
> Si l'automontage de clé USB a été ajouté au système lors de la préparation de l'image système, copier le firmware sur l'emplacement émulant le stockage du RP2040:
>
> ```
> cp ~/klipper/out/klipper.uf2 ~/printer_data/gcodes/USB
> sync
> ```
> on peut également faire un
>  ```
> cat ~/klipper/out/klipper.uf2 ~/printer_data/gcodes/USB
> sync
> ```

- Au cas où l'automontage n'a pas été installé, il faudra d'abord monter le stockage (*nécessite les droits root*):
```
sudo mount /dev/sda1 /mnt
sudo systemctl daemon-reload
```
Puis procéder au «flashage» via copie du firmware
```
sudo cp /home/mks/klipper/out/klipper.uf2 /mnt
sync
sudo umount /mnt
```

> [!NOTE]
> Une fois le fichier .uf2 copié, le RP2040 se déconnectera automatiquement en tant que périphérique de stockage de masse USB.
> Par précaution, on démonte tout de même manuellement.
>
> Un `lsusb` permet de vérifier que le RP2040 n'est plus en mode émulation de stockage
> 
> <p align="center"><img src="/Images/lsusb-rp2040-openmoko.jpg"></p>

## Méthode 2

### 1 - Firmware Katapult (installer, préparer, flasher)

Connecté en ssh, lancer la suite de commandes:

- Cloner le dépôt :

`git clone https://github.com/Arksine/katapult`
- préparer la configuration

```
cd ~/katapult
make menuconfig
```

- choisir les options
  - [X] Raspberry Pi RP2040
  - [X] build Katapult deployment application (16 KiB booloader)
  - [X] communication interface (USB)

<details>
<summary>Choix à réaliser</summary>
 
<p align="center">
<img src="/Images/katapult-rp2040.jpg">
<img src="/Images/katapult-16k-bootloader.jpg">
<img src="/Images/katapult-menuconfig.jpg">
</p>

</details>

- Presser <kbd>Q</kbd> puis <kbd>Y</kbd>(es) pour sauvegarder cette configuration
- compiler le firmware Katapult

```
make clean
make -j4
```

- A l'issue de la compilation, le firmware Katapult est prêt à être installé, il se trouve dans le dossier ~/katapult/out et porte le nom **katapult.uf2**
<details>
<summary>Fin de la compilation</summary>
 
<p align="center">
<img src="/Images/katapult-compil-rp2040.jpg">
</p>

</details>

- l'installation du firmware katapult.uf2 est similaire à l'installation de klipper.uf2 utilisé avec la **méthode 1**

<details>
<summary>Flasher katapult.uf2</summary>

Pour flasher ce firmware, le contrôleur RP2040 doit passer en mode émulation du stockage (BOOTSEL mode).

- éteindre l'imprimante et patienter au moins 30 secondes le temps que le supercondensateur se décharge complètement.
- le capot arrière de la tête étant démonté:
  - presser et maintenir enfoncé le bouton au bas de la carte nommé **BOOT**
  - allumer l'imprimante
  - **Ne pas relâcher la pression sur ce bouton  tant que l'imprimante n'a pas complètement démarré.** 
<p align="center">
<img src="/Images/toolhead.jpg">
</p>

- relâcher le bouton BOOT quand la lumière interne de l'imprimante s'allume ou une fois l'écran affichant un problème de démarrage (le système d'exploitation ne comporte plus les logiciels permettant la communication entre la carte et l'écran => le firmware de l'écran considère qu'il y a un problème 😏)
- se (re)connecter en ssh en utilisateur ***mks***
- vérifier que le RP2040 est bien en mode émulation de stockage :
  - `lsblk` doit afficher un périphérique sda (partition sda1)
  - et/ou avec `lsusb` indiquant que le RP2040 est passé dans le «bon» mode (**ID 2a8a:0003 Raspberry Pi RP2 Boot**) 
- Si aucun périphérique sda1 n'apparait à la suite de la commande `lsblk` (et/ou `lsusb`), c'est que le RP2040 n'est pas passé en mode émulation de stockage de masse (BOOTSEL mode):
  - presser en maintenant enfoncé le bouton BOOT,
  - presser et relâcher le bouton RESET,
  - relâcher alors le bouton BOOT.
  - vérifier à nouveau avec un `lsblk` (et/ou via `lsusb`) le passage en mode BOOTSEL
- Si l'automontage de clé USB a été ajouté au système, copier le firmware sur l'emplacement émulant le stockage du RP2040:

```
cp ~/katapult/out/katapult.uf2 ~/printer_data/gcodes/USB
sync
```

- Sinon, il faudra procéder au montage manuel du stockage :
```
sudo mount /dev/sda1 /mnt
sudo systemctl daemon-reload
```
Puis procéder au «flashage» via copie du firmware
```
sudo cp /home/mks/katapult/out/katapult.uf2 /mnt
sync
sudo umount /mnt
```

</details>

- une fois katapult installé comme chargeur de démarrage, reste à compiler le firmware Klipper et à l'installer

> [!IMPORTANT]
> Katapult installé comme chargeur de démarrage permet désormais de ne plus avoir à ouvrir le capot arrière pour pouvoir déclencher le mode émulation de stockage de masse (BOOTSEL mode) du Raspberry Pi RP2040

### 2 - Firmware Klipper via Katapult (préparer, flasher)

- la préparation du firmware Klipper est similaire à la méthode 1, **la seule différence étant d'indiquer que Klipper doit s'installer avec un décalage en mémoire prenant en compte le chargeur de démarrage (bootloader) de Katapult**

```
cd ~/klipper
make menuconfig
``` 

- Le menu de configuration du firmware apparait, choisir les options :
  - [X] cocher «Enable extra low-level»
  - [X] RP2040 comme contrôleur
  - [X] **chargeur de démarrage 16 Kio**
  - [X] USB comme interface de communication

<details>
<summary>RP2040, bootloader de 16 Kio</summary>

<p align="center">
<img src="/Images/klipper-menuconfig-16k-bootloader-rp2040.jpg">
</p>

pour obtenir au final

<p align="center">
<img src="/Images/klipper-menuconfig-rp2040-katapult.jpg">
</p>
 
</details>  

- une fois ces options sélectionnées, presser <kbd>Q</kbd> pour sortir de ce menu, valider par <kbd>Y</kbd> pour sauvegarder la configuration
- compiler Klipper

```
make clean
make -j4
```

- attendre que le processus se termine
<details>
<summary>Fin de la compilation</summary>

<p align="center">
<img src="/Images/klipper-compil-rp2040.jpg">
</p>

</details>  

Le firmware a été compilé dans le dossier ~/klipper/out et porte cette fois le nom **klipper.bin**

Pour permettre le flashage via Katapult, un paquet Python doit être installé :

`sudo apt install python3-serial`

Le flashage est effectué via USB en utilisant le script `flashtool.py` fourni par Katapult. Il nécessite en paramètre le périphérique série indiqué par `ls /dev/serial/by-id` (penser à le copier pour ensuite le coller après l'option (-d)).

Utiliser la commande suivante :

`python3 ~/katapult/scripts/flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/usb-katapult_rp2040_xxxxxxxxxxxxxx`

Remplacer ci-dessus dans `/dev/serial/by-id/by-id/usb-katapult_rp2040_xxxxxxxxxxxxxx`, les ***xxxxxxxxxxx*** par le nombre retourné sur votre système (ou effacer ce /dev/serial/by-id/by-id/usb-katapult_rp2040_xxxxxxxxxxxxxx et coller celui obtenu avec ls /dev/serial/by-id).

---
> [!NOTE]
> 
> Les firmwares Klipper sont maintenant tous installés sur les différents contrôleurs dans des versions identiques.
> 
> <p align="center"><img src="/Images/fluidd-mcus-v0.12.jpg"></p>
> <p align="center"><img src="/Images/mainsail-mcus-v0.12.jpg"></p>
>    
> Ni Fluidd ni Mainsail n'indiquent plus d'erreurs

---
Dernière étape, le fichier de configuration **printer.cfg**, voir [ici](../Configurations/printer-cfg.md)

> [!TIP]
>
> Pour palier au non fonctionnement de l'écran tactile, on peut y remédier de plusieurs manières.
> Pour en savoir plus, consulter [ce document](../Hardware/ecran-tactile.md)


