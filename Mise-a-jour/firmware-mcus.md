# PRÉPARATION / INSTALLATION (flashage) du FIRMWARE KLIPPER

## SOURCES

Les sources m'ayant servi à écrire les différents tutoriels :
- Mark Ewert (@melvenx), a réalisé [ce document PDF](https://github.com/melvenx/QIDI/blob/main/How%20to%20Update%20Klipper%20for%20the%20QIDI%20Tech%20XMAX3%20XPLUS3%20XSMART3%20Printers%20v2.pdf) relatant les étapes pour flasher les firmwares. Il est le premier, à ma connaissance, à avoir révéler le nom à utiliser pour flasher Klipper sur le micro-contrôleur STM32F401 ( **X_4.bin** ), étape primordiale pour réussir les flashages divers.
- Un site [OpenQIDI](https://openqidi.com) avait mis à disposition une documentation (livre) supprimée depuis par l'utilisateur @phill1988 qui a remis cette documentation sur [ce dépôt FreeQidi](https://github.com/Phil1988/FreeQIDI)
- @leadustin a ensuite fourni une documentation plus étoffée d'abord en langue allemande puis [en anglais](https://github.com/leadustin/QIDI-up2date-english)
- un des développeurs chargé chez Qidi de l'écosystème Klipper ( @cchen616 ) a décrit les étapes essentielles dans [cette issue Github](https://github.com/QIDITECH/QIDI_PLUS3/issues/27#issuecomment-2073932891)

## FIRMWARE

Pour chacun des contrôleurs (principal (MCU), secondaire (la tête «MKS-THR») et le Linux MCU), les étapes à suivre sont indentiques:
- se placer dans le répertoire Klipper du dossier personnel de l'utilisateur «mks»

`cd ~/klipper` ou `cd /home/mks/klipper` ou si on est déjà dans le dossier perso `cd`
- configurer le firmware

`make menuconfig`
- dans la fenêtre de configuration, sélectionner les paramètres adéquats
- préparer la compilation

 `make clean`
- compiler

`make` ou mieux puisque le microcontrôleur RK3328 possède plusieurs coeurs, utiliser une compilation parallèle `make -j4`
- à l'issue de la compilation, récupérer le firmware dans le dossier **out** (~/klipper/out/):
  - klipper.bin ou
  - klipper.uf2 ou
  - klipper.elf
- flasher ce firmware sur le MCU ( la méthode dépend du MCU à flasher )
  - MCU Principal (STM32F402)
    - recopier le fichier out/klipper.bin à la racine d'une carte SD sous le nom **X_4.bin** (X majuscule, souligné, quatre, point, b, i, n)
    - éteindre l'imprimante, attendre la décharge des Supercondensateurs installés sur la carte Qidi (au moins 30 secondes), insérer la carte SD contenant le fichier X_4.bin, allumer => le processus de flashage est très rapide (moins de trente secondes)
  - MCU secondaire (tête MKS-THR, microcontrôleur RP2040)
