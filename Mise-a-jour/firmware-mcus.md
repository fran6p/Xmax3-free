# PRÉPARATION / INSTALLATION (flashage) du FIRMWARE KLIPPER

## SOURCES

Les sources m'ayant servi à écrire les différents tutoriels :
- Mark Ewert (@melvenx), a réalisé [ce document PDF](https://github.com/melvenx/QIDI/blob/main/How%20to%20Update%20Klipper%20for%20the%20QIDI%20Tech%20XMAX3%20XPLUS3%20XSMART3%20Printers%20v2.pdf) relatant les étapes pour flasher les firmwares. Il est le premier, à ma connaissance, à avoir révéler le nom à utiliser pour flasher Klipper sur le micro-contrôleur STM32F401 ( **X_4.bin** ), étape primordiale pour réussir les flashages divers.
- Un site [OpenQIDI](https://openqidi.com) avait mis à disposition une documentation (livre) supprimée depuis par l'utilisateur @phill1988 qui a remis cette documentation sur [ce dépôt FreeQidi](https://github.com/Phil1988/FreeQIDI)
- @leadustin a ensuite fourni une documentation plus étoffée d'abord en langue allemande puis [en anglais](https://github.com/leadustin/QIDI-up2date-english)
- un des développeurs chargé chez Qidi de l'écosystème Klipper ( @cchen616 ) a décrit les étapes essentielles dans [cette issue Github](https://github.com/QIDITECH/QIDI_PLUS3/issues/27#issuecomment-2073932891)

## FIRMWARE GÉNÉRALITÉS

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

Connaissant le principe, on va pouvoir entrer dans les détails.

## PRÉREQUIS

Quelques outils sont nécessaires:
- clé Allen (Hexagonale) 2.0 mm
- carte μSD, clé USB ou adaptateur USB/μSD
- logiciels:
  - accès distant (SSH)
  - permettant le transfert de fichiers entre matériels en réseau
 
## PRÉPARATION MATÉRIELLE

> [!NOTE]
>
> Imprimante éteinte
> 

- accéder à l'arrière de l'imprimante pour retirer les vis du capot renfermant l'électronique à l'aide de la clé Allen de 2.0 mm
- retirer ce capot (le ventilateur de refroidissement de la carte y est fixé)
- toujours à l'aide de la clé Allen de 2.0mm, retirer les quatre vis de fication du capot arrière de la tête

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
  
