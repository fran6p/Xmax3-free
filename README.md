# X-MAX 3 - FREE

Le but de ces tutoriels est de fournir un pas-à-pas permettant à un utilisateur de mettre à niveau les logiciels y compris le système d'exploitation de son imprimante.

Les imprimantes Qidi Series 3 utilisent une base matérielle solide offrant de bons potentiels mais dont les briques logicielles sont «dépassées».

## FONCTIONNALITÉS

- dernière version Armbian: Bookworm
- derniers noyaux (Linux kernel): 6.6.x
- Python (3.12)
- briques essentielles installées (écosystème Klipper) :
  - Klipper (0.12.x)
  - Moonraker (0.8.x)
  - Fluidd (port 10088)
  - Mainsail (port http standard (80))
  - Crowsnest (gestionnaire de caméras)
  - Optionnels mais forts pratiques:
    - Timelapses
    - KAMP (uniquement pour lignes de purge, Klipper 0.12.x le gérant désormais nativement)
    - Shake&Tune
    - …
- fichier de configuration (printer.cfg) «amélioré» (réparti sur plusieurs fichiers via des inclusions)
- macros supplémentaires

> [!IMPORTANT]
>
> Toutes les fonctionnalités de l'imprimante d'origine sont préservées **sauf**
> - écran tactile originel
> - ~~Wifi~~ (X-Max3 / X-Plus3 peuvent heureusement communiquer en Ethernet)

## COMMENT PROCÉDER

Débutez par ce [premier guide](./Mise-a-jour/installation-OS.md).

> [!NOTE]
> Ce projet est toujours en cours d'améliorations, présence de «bogues« possibles 😏
> 
> Comme tout projet Github, vous êtes bienvenus pour remonter des «issues» et pourquoi pas proposer des «pull requests»
> 


