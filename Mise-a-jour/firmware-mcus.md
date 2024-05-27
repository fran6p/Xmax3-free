# PRÉPARATION / INSTALLATION du FIRMWARE KLIPPER

## SOURCES

Les sources m'ayant servi à la rédaction des tutoriels :
- Mark Ewert, @melvenx, a réalisé [ce document PDF](https://github.com/melvenx/QIDI/blob/main/How%20to%20Update%20Klipper%20for%20the%20QIDI%20Tech%20XMAX3%20XPLUS3%20XSMART3%20Printers%20v2.pdf) relatant les étapes pour flasher les firmwares avec l'OS originel de Qidi (Armbian Buster de Makerbase)). Il est le premier, à ma connaissance, à avoir révéler le nom «spécifique» du fichier à utiliser pour flasher Klipper sur le micro-contrôleur STM32F401 ( **X_4.bin** ), étape primordiale pour réussir les flashages divers.
- Un site [OpenQIDI](https://openqidi.com) avait, dans un premier temps, mis à disposition une documentation (livre) supprimée depuis par l'utilisateur @phill1988 qui a remis à disposition cette documentation sur ce dépôt [FreeQidi](https://github.com/Phil1988/FreeQIDI)
- @leadustin a ensuite fourni une documentation plus étoffée d'abord en langue allemande puis [en anglais](https://github.com/leadustin/QIDI-up2date-english)
- un des développeurs chargé chez Qidi de l'écosystème Klipper ( @cchen616 ) décrit les étapes essentielles dans [cette issue Github](https://github.com/QIDITECH/QIDI_PLUS3/issues/27#issuecomment-2073932891) (à partir de l'OS originel (Armbian buster)).

## FIRMWARE GÉNÉRALITÉS

Pour chacun des contrôleurs :
- principal (MCU),
- secondaire (la tête «MKS-THR»)
- le Linux MCU), les étapes à suivre sont quasi identiques:

- se placer dans le répertoire Klipper du dossier personnel de l'utilisateur «mks»

`cd ~/klipper` ou `cd /home/mks/klipper` ou si on est déjà dans le dossier perso `cd klipper` ou `cd ./klipper`
- configurer le firmware

`make menuconfig`
- dans la fenêtre de configuration, sélectionner les paramètres adéquats (dépend de chaque MCU)
- préparer la compilation

 `make clean`
- compiler

`make`

ou encore, le microcontrôleur RK3328 possèdant plusieurs coeurs, utiliser une compilation parallèle

`make -j4`
- à l'issue de la compilation, récupérer le firmware dans le dossier **out** (~/klipper/out/). Le nom du fichier compilé dépend des directives de compilation ( klipper.bin / klipper.uf2 / klipper.elf )
- flasher ce firmware sur le MCU ( la méthode dépend du MCU à flasher )

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
  
## MCU PRINCIPAL

## MCU Linux (controleur de la carte X-4 / X-6, Rockchip RK3328)

## MCU tête (carte A-4. controleur RP2040)

Ici, le firmware Klipper peut être installé de deux façons:
1. la première nécessitera **à chaque flashage / reflashage le démontage du capot arrière de la tête** pour accéder au bouton poussoir «BOOTSEL» permettant de passer le controleur RP2040 en mode émulation de stockage
2. la seconde ne demandera l'étape ci-dessus qu'**une seule fois** pour l'installation d'un chargeur de démarrage permettant ensuite le flashage du controleur RP2040 via USB. Elle nécessite par contre l'installation supplémentaire du logiciel KATAPULT (ex CANBOOT) de @arksine

### Méthode 1

Connecté en ssh, lancer la suite de commandes:
```
cd ~/klipper
make clean
make menuconfig
```
Le menu de configuration du firmware apparait, choisir les options :
- cocher «Enable extra low-level»
- RP2040 comme contrôleur
<details>
<summary>choix RP2040</summary>
 
![Raspberry Pi RP2040](../Images/klipper-menuconfig-choix-rp2040.jpg)

</details>

- **Pas de chargeur de démarrage**
- USB comme interface de communication

<details>
<summary>Au final</summary>

![Raspberry Pi RP2040](../Images/klipper-menuconfig-rp2040.jpg)
 
</details>

- une fois ces options sélectionnées, presser Q pour sortir de ce menu, valider par Y pour sauvegarder la configuration

![sauvegarder-configuration](../Images/make-menuconfig-save.jpg)

- compiler le firmware `make` ou en profitant de plusieurs coeurs du contrôleur RK3328 `make -j4`
- attendre que le procesus se termine
<details>
<summary>Extrait de la compilation</summary>

 ![extrait](../Images/klipper-compil-rp2040-uf2.jpg)

</details>  

Le firmware a été compilé dans le dossier ~/klipper/out et porte le nom **klipper.uf2**

### Flasher le firmware klipper.uf2

**Pour flasher ce firmware, le contrôleur RP2040 doit passer en mode émulation du stockage (BOOTSEL mode)**.
- éteindre l'imprimante et patienter au moins 30 secondes le temps que le supercondensateur se décharge complètement.
- le capot arrière de la tête étant démonté:
  - presser et maintenir enfoncé le bouton au bas de la carte nommé **BOOT**
  - allumer l'imprimante
  - **ne relâcher la pression sur ce bouton qu'une fois l'imprimante complètement démarrée. 

![bootsel](../Images/toolhead.jpg)
- relâcher le bouton BOOT quand la lumière interne de l'imprimante s'allume ou une fois l'écran affichant un problème de démarrage (le système d'exploitation ne comporte plus les logiciels permettant la communication entre la carte, le firmware de l'écran considère qu'il y a un problème 😏)
- se (re)connecter en ssh en utilisateur ***mks***
- vérifier que le RP2040 est bien en mode émulation de stockage `lsblk` doit afficher un périphérique sda (partition sda1), un `lsusb` permet également de vérifier que le RP2040 est passé dans le «bon» mode (**ID 2a8a:0003 Raspberry Pi RP2 Boot**):
![lsusb](./Images/rp2040-lsusb-boot.jpg)
- Si aucun périphérique sda1 n'apparait à la suite de la commande `lsblk` ou que le périphérique USB n'est pas `ID 2a8a:0003 Raspberry Pi RP2 Boot`:
  - presser et maintenir enfoncé le bouton BOOT,
  - presser et relâcher le bouton RESET,
  - relâcher alors le bouton BOOT.
  - vérifier à nouveau avec un `lsblk` et/ou un `lsusb`
- Si l'automontage de clé USB a été ajouté au système, copier le firmware sur l'emplacement émulant le stockage du RP2040:
  - `cp ~/klipper/out/klipper.uf2 ~/printer_data/gcodes/USB`
  - on peut également faire un `cat ~/klipper/out/klipper.uf2 ~/printer_data/gcodes/USB`

- Sinon, il faudra d'abord monter le stockage :
```
sudo mount /dev/sda1 /mnt
sudo systemctl daemon-relaod
```
Puis procéder au «flashage» via copie du firmware
```
sudo cp /home/mks/klipper/out/*klipper.uf2 /mnt
sync
sudo umount /mnt
```

> [!NOTE]
> Une fois le fichier .uf2 copié, le RP2040 se déconnecte automatiquement en tant que périphérique de stockage de masse USB et exécute le code.
> Par précaution, on démonte tout de même manuellement.
>
> Un `lsusb` permet de vérifier que le RP2040 n'est plus en mode émulation de stockage
> ![](./images/lsusb-rp2040-openmoko.jpg)

### Méthode 2

Connecté en ssh, lancer la suite de commandes:

#### Installation de KATAPULT (ex CANBOOT)

- Cloner le dépôt :

`git clone https://github.com/Arksine/katapult`
- préparer la configuration

```
cd ~/katapult
make menuconfig
```

- choisir les options
  - Raspberry Pi RP2040
  - build Katapult deployment application (16 KiB booloader)
  - communication interface (USB)

<details>
<summary>choix à réaliser</summary>
 
![katapult](../Images/katapult-rp2040.jpg)
![katapult](../Images/katapult-16k-bootloader.jpg)

Au final

![katapult](../Images/katapult-menuconfig.jpg)

</details>

- Presser Q puis Yes pour sauvegarder la configuration
- compiler le firmware Klipper

```
make clean
make -j4
```

- A l'issue de la compilation, le firmware Katapult est prêt à être installé, il se trouve dans le dossier ~/katapult/out et porte le nom **katapult.uf2**
<details>
<summary>résultat de la compilation</summary>
 
![katapult](../Images/katapult-compil-rp2040.jpg)

</details>

- l'installation du firmware katapult.uf2 est similaire à l'installation de klipper.uf2 de la **méthode 1**
<details>
<summary>Flasher katapult.uf2</summary>

Pour flasher ce firmware, le contrôleur RP2040 doit passer en mode émulation du stockage (DFU mode).
- éteindre l'imprimante et patienter au moins 30 secondes le temps que le supercondensateur se décharge complètement.
- le capot arrière de la tête étant démonté, presser et maintenir enfoncé le bouton au bas de la carte nommé **BOOT** puis allumer l'imprimante.
**Ne pas relâcher la pression sur ce bouton  tant que l'imprimante n'a pas complètement démarré.** 

![bootsel](../Images/toolhead.jpg)
- Relâcher le bouton BOOT quand la lumière interne de l'imprimante s'allume ou une fois l'écran affichant un problème de démarrage (le système d'exploitation ne comporte plus les logiciels permettant la communication entre la carte et l'écran et donc le firmware de l'écran considère qu'il y a un problème 😏)
- Se (re)connecter en ssh en utilisateur ***mks***
- Vérifier que le RP2040 est bien en mode émulation de stockage `lsblk` doit afficher un périphérique sda (partition sda1), éventuellement sdb (sdb1)
- Si aucun périphérique sda1 (sdb1) n'apparait à la suite de la commande `lsblk`, c'est que le RP2040 n'est pas passé en mode DFU:
  - presser en maintenant enfoncé le bouton BOOT,
  - presser et relâcher le bouton RESET,
  - relâcher alors le bouton BOOT.
  - vérifier à nouveau avec un `lsblk` le passage en mode DFU
- Si l'automontage de clé USB a été ajouté au système, copier le firmware sur l'emplacement émulant le stockage du RP2040:

`cp ~/katapult/out/katapult.uf2 ~/printer_data/gcodes/USB`

- Sinon, il faudra procéder au montage manuel du stockage :
```
sudo mount /dev/sda1 /mnt
sudo systemctl daemon-relaod
```
Puis procéder au «flashage» via copie du firmware
```
sudo cp /home/mks/katapult/out/katapult.uf2 /mnt
sudo umount /mnt
```

</details>

- une fois katapult installé comme chargeur de démarrage, reste à compiler le firmware Klipper et à l'installer

> Katapult installé comme chargeur de démarrage permet désormais de ne plus avoir à ouvrir le capot arrière pour pouvoir déclencher le mode DFU du Raspberry Pi RP2040

#### Installer Klipper sur la carte A-4 via l'aide de Katapult

- la préparation du firmware Klipper est similaire à la méthode 1, **la seule différence étant d'indiquer que Klipper doit s'installer avec un décalage en mémoire prenant en compte le chargeur de démarrage (bootloader) de Katapult**

```
cd ~/klipper
make menuconfig
``` 

- Le menu de configuration du firmware apparait, choisir les options :
  - cocher «Enable extra low-level»
  - RP2040 comme contrôleur
  - **chargeur de démarrage 16 Kio**
  - USB comme interface de communication

<details>
<summary>RP2040, bootloader de 16 Kio</summary>

![bootloader](../Images/klipper-menuconfig-16k-bootloader-rp2040.jpg)

pour obtenir au final

![config](../Images/klipper-menuconfig-rp2040-katapult.jpg)
 
</details>  

- une fois ces options sélectionnées, presser Q pour sortir de ce menu, valider par Y pour sauvegarder la configuration
- compiler Klipper

```
make clean
make -j4
```

- attendre que le proccesus se termine
<details>
<summary>Extrait de la compilation</summary>

 ![extrait](../Images/klipper-compil-rp2040.jpg)

</details>  

Le firmware a été compilé dans le dossier ~/klipper/out et porte cette fois le nom **klipper.bin**

##### Flasher le firmware klipper.bin

Pour permettre le flashage via Katapult, un paquet Python doit être installé :

`sudo apt install python3-serial`

Le flashage va être effectué via USB en utilisant le périphérique série indiqué par `ls /dev/serial/by-id` avec la commande :

`python3 ~/katapult/scripts/flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/usb-katapult_rp2040_xxxxxxxxxxxxxx`

Remplacer évidemment les «xxxxxxxxxxx» par le nombre retourné sur votre système.

> [!NOTE]
> Les firmwares Klipper sont maintenant tous installés sur les différents contrôleurs dans des versions identiques.
> Ni Fluidd ni Mainsail n'indiquent plus d'erreurs

copies écrans

Dernière étape, le fichier de configuration **printer.cfg**, voir [ici](./printer-cfg.md)


