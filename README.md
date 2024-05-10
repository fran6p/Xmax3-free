# X-MAX 3 - FREE

Le but de ce projet est de créer un environnement logiciel basé sur des composants logiciels dans le total respect de leurs licences, incluant entre autre :
- toutes les fonctionnalités du firmware originel
- les dernières versions de Klipper, Moonraker, Fluidd, Mainsail, …
- une version la plus récente du système d'exloitation (OS), Armbian (basée sur Debian) «Bookworm» et noyau (kernel) récent pour offrir une sécurité accrue
- configuration et macros «améliorées»

## FONCTIONNALITÉS

- dernière version Armbian: Bookworm
- derniers noyaux (Linux kernel): 6.6.x
- briques essentielles installées (écosystème Klipper) :
  - Klipper (0.12.x)
  - Moonraker (0.8.x)
  - Fluidd (port 10088)
  - Mainsail (port http standard (80))
  - Crowsnest (gestionnaire de caméras)
  - KAMP (uniquement pour lignes de purge, Klipper 0.12.x le gérant désormais nativement)
  - Shake&Tune
  - …
- fichier de configuration (printer.cfg) «amélioré» (réparti sur plusieurs fichiers via des inclusions)
- macros supplémentaires

> [!IMPORTANT]
>
> Toutes les fonctionnalités de l'imprimante d'origine sont préservées **sauf**
> - écran tactile originel
> - Wifi (X-Max3 / X-Plus3 peuvent heureusement communiquer en Ethernet)

## COMMENT PROCÉDER

Commencer en débutant par ce [premier guide](./Mise-a-jour/installation-OS.md).

> [?NOTE]
> Ce projet est toujours en phase de test, des bogues peuvent être présents 😏
> Comme tout projet Github, vous êtes bienvenus pour rapport des «issues» et pourquoi pas proposer des «pull requests»


