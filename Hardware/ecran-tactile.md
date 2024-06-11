# ÉCRAN TACTILE

## Écran originel

L'écran installé sur l'imprimante est un écran type HMI (Human Machine Interface), **en français IHM (interface Homme Machine)**, plusieurs fabricants en proposent, Nextion et sa variante asiatique TJC sont les plus connus.

Ces écrans HMI sont des **ordinateurs monocartes à part entière, fonctionnant avec leur propre micrologiciel** (firmware).  Ils ne sont pas spécifiques aux imprimantes Qidi, le micrologiciel qui les fait fonctionner lui par contre a été programmé spécifiquement pour chaque modèle.

Ce type d'écran communique avec la carte contrôleur en échangeant des informations avec celle-ci par le biais d'événements.  Ce n'est donc pas comme un écran HDMI qui lui affiche une image en fonction du signal envoyé.    
Il s'agit plutôt d'une transmission de messages, par exemple :
- la carte contrôleur envoie un message X à l'écran, X étant l'écran de chargement du filament ou
- l'écran envoie un message à la carte contrôleur que le bouton 007 a été enfoncé, ce bouton 007 correspondant au bouton pour extruder le filament de 20 mm.

Pour que la communication se fasse entre carte et écran, un logiciel est nécessaire: ***Xindi***

Ce programme d'interface d'écran de Qidi agit comme un intermédiaire en convertissant les messages de type IHM (Interface Homme Machine) en commandes à envoyer à Moonraker/Klipper, et vice-versa.

La communication entre l'écran et la carte est de type série ( [UART](https://fr.wikipedia.org/wiki/UART) ), son débit de données est lent, à 115200 bauds, soit 115200 bits/s (≃ 14400 octets/s (8N1, 8 bits, pas de parité, 1 bit de stop).

c'est pourquoi les mises à jour du firmware de l'écran sont si lentes. Le micrologiciel de l'écran pèse environ 20 Mo, pour chaque mise à jour du firmware de l'écran, la carte de contrôle doit envoyer 20 Mo sur une liaison de 14,4 Ko/s, ce qui prend une bonne demi-heure (*on peut aller plus vite en utilisant une carte μSD sur laquelle a été copié le firmware écran introduite dans le lecteur SD à l'arrière de l'écran*). Les autres mises à jour de Qidi sur la carte contrôleur principale sont effectuées en moins de 5 secondes.

## Pourquoi remplacer l'écran ?

Comme on l'a vu ci-dessus, pour fonctionner, le firmware de l'écran originel doit pouvoir communiquer avec Moonraker / Klipper par l'intermédiaire d'un programme (Xindi). En installant une image du système plus récente (Armbian Bookworm + écosystème Klipper «vanilla»), ce programme n'a pas été réinstallé.

Xindi et le microgiciel de l'écran utilisent des méthodes / événements codés en «dur» (certains chemins par exemple sont différents (configuration de Klipper, journaux, Gcodes) entre l'écosystème Klipper «officiel» et celui de Qidi), le Zoffset est géré par l'écran, sa valeur est stockée dans le fichier ***config.mksini***, des fichiers Python de Klipper et Moonraker ont été modifiés pour pouvoir gérer cet écran.

Donc, le simple ajout de Xindi ne permettrait pas le fonctionnement correct de l'écran. Il faudrait re-programmer tous les évènements. Ce serait éventuellement faisable pour quelqu'un de très motivé. Nextion commercialise deux versions de sa gamme de produits:
- les cartes NX, pour le marché mondial hors Chine,
- les cartes TJC pour la Chine continentale.

Le hardware des cartes est essentiellement le même. Les cartes TJC étant environ deux fois moins chères que les cartes NX, pour protéger son marché à deux niveaux, Nextion ne propose l'éditeur TJC qu'en langue chinoise.

Pour ceux qui seraient tentés, les dépôts Github de Qidi ont été mis à jour pour la branche 4.x.13 dans lesquels un dossier UI donne accès aux fichiers xxxxx.hmi qui une fois compilés donnent le firmware écran à flasher ( xxxxx.tft) => [Xmax3]( Il faudrait re-programmer tous les évènements. Ce serait éventuellement faisable pour quelqu'un de très motivé (ce qui n'est pas mon cas 😉 ). Nextion commercialise deux versions de sa gamme de produits. Il y a les cartes NX, pour le marché mondial hors Chine, et les cartes TJC pour la Chine continentale. Les cartes sont essentiellement les mêmes. Les cartes TJC sont environ deux fois moins chères que les cartes NX. Pour protéger le marché à deux niveaux, Nextion ne propose l'éditeur TJC qu'en langue chinoise.

Pour ceux qui seraient tentés, les dépôts Github de Qidi ont été mis à jour pour la branche 4.x.13 dans lesquels un dossier UI donne accès aux fichiers xxxxx.hmi qui une fois compilés donnent le firmware écran à flasher ( xxxxx.tft) => [Xmax3](https://github.com/QIDITECH/QIDI_MAX3/tree/MAX3_V4.3.13/UI), [Xplus3](https://github.com/QIDITECH/QIDI_PLUS3/tree/PLUS_V4.2.13/UI) 

<details>
 <summary>Pour en savoir plus</summary>

- [](https://hackaday.io/page/1652-nextion-hmi-display-a-user-interface-that-is-simple-and-easy-to-use)
- [](https://www.lyonscomputer.com.au/Electronic-Devices/Nextion-HMI-LCD-Display/Nextion-HMI-LCD-Display.html)
 
</details>
Au premier allumage de l'imprimante après installation du nouveau système, le firmware autonome de l'écran ne pouvant communiquer avec la carte, provoque cet affichage sur l'écran
 
<p align="center">
<img src="/Images/BSOD-screen.jpg">
</p>

Contrairement à la carte MKS SKIPR de Makerbase sur laquelle les cartes contrôleur Qidi (X-4, X-6) sont basées, ces dernières ne possèdent aucun moyen matériel d'installer un écran:
- pas de sortie HDMI,
- pas de connecteur prévu pour un écran LCD

En l'absence de connecteur, utiliser un autre écran va nécessiter l'utilisation de matériels supplémentaires.

## Que choisir ?

Plusieurs possibilités:
1. utiliser un smartphone / tablette et y installer une application type [Mobileraker](https://play.google.com/store/apps/details?id=com.mobileraker.android) / [Klipperoid](https://play.google.com/store/apps/details?id=com.didwkrrk.klipperoid), voir >>> [ici](https://www.lesimprimantes3d.fr/forum/topic/51248-klipper-sur-tablette-android/) <<<
2. utiliser un smartphone / tablette avec Klipperscreen installé sur la carte contrôleur et un serveur X installé côté téléphone (connexion via ADB), voir [cette documentation de KlipperScreen]() ou via VNC (serveur TigerVNC + client VNC (AVNC) sur la tablette / smartphone)
3. utiliser un mini écran (2,8 ou 3,5") sur lequel est installé / flashé CYD-Klipper, voir >>> [ici](https://www.lesimprimantes3d.fr/forum/topic/56853-klipper-%C3%A9cran-tactile-minimaliste-et-bon-march%C3%A9/) <<<
4. se passer de l'écran et n'utiliser que l'ordinateur via une interface Web ( Fluidd / Mainsail )
5. utiliser un autre écran tactile de 5", relié en HDMI ou via une nappe DSI à un ordinateur monocarte (SBC) genre Raspberry Pi (minima, un RPi 0v2)

C'est cette dernière option qui sera décrite ci-dessous

## Matériels nécessaires

- un écran tactile, par exemple celui-ci de Bigtreetech ( [BTT HDMI 5"](https://li3dforum.ovh/bMV) , [manuel](https://github.com/bigtreetech/BIGTREETECH-TouchScreenHardware/blob/master/BTT%20HDMI5/Hardware/BIGTREETECH%20HDMI5%20V1.0%20User%20Manual.pdf) )
- un SBC, par exemple un [Raspberry Pi 0 v2](https://www.kubii.com/fr/cartes-nano-ordinateurs/3455-raspberry-pi-zero-2-w-5056561800004.html)
- un câble HDMI avec les bons connecteurs ou pour le côté RPi0v2 un adaptateur HDMI femelle vers HDMI mini mâle
- un câble USB pour la gestion du tactile
- une alimentation 5V
- une carte μSD (8 Go ou plus)
- une clé Allen 2mm pour démonter l'ancien écran

## Logiciel nécessaire

- [Raspberry Pi Imager](https://www.raspberrypi.com/software/)

### Installation du système Raspberry Pi OS

Je ne décris pas en détails la procédure (voir [ici](https://www.lesimprimantes3d.fr/forum/topic/50322-klipper-lutiliser-en-toute-connaissance-de-cause/?do=findComment&comment=521329) pour une installation «imagée»).

### Les étapes essentielles

- Installer une image de base (Raspberry Pi OS lite, Bookworm, 64 bits) via Raspberry Pi Imager. Lors de la préparation de l'image avec RaspberryPi Imager, **penser à ajouter un utilisateur «mks» avec son mot de passe «makerbase»**

En fonction de l'écran HDMI utilisé, il faut réaliser quelques ajustements dans le fichier config.txt (**/boot/firmware/config.txt**).  Le dossier /boot est sur une partition en FAT32, on peut donc y accéder pour éditer ce fichier.

Pour l'écran Bigtreetech, voici les lignes ajoutées en fin de fichier (si vous utilisez un autre écran, veuillez consulter le site web du fabricant) :  
```
# BTT HDMI5
hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=2
```

Enregistrer ces modifications, éjecter proprement la carte SD
- démarrer le Raspberry Pi 0v2 avec la carte μSD sur laquelle l'OS a été installé
- récupérer par tout moyen approprié l'adresse IP du RPi0v2 et s'y connecter en ssh
- faire les mises à jour
- installer git (normalement déjà présent mais ça ne peut pas faire de mal)
- comme on n'installe pas Klipper, créer les dossiers printer_data/config et printer_data/logs
- cloner le dépôt KIAUH
- via son intermédiaire, installer Klipperscreen
- quitter KIAUH
- créer le fichier de configuration KlipperScreen.conf ( K et S en majuscules 😉 )
- redémarrer le RPi

```
sudo apt update && sudo apt upgrade -y
sudo install -y git
mkdir -p ~/printer_data/config ~/printer_data/logs
git clone https://github.com/dw-0/kiauh
./kiauh/kiauh.sh
…  (installer  Klipperscreen, puis quitter KIAUH)
cd printer_data/config
nano KlipperScreen.conf
… (ajouter les lignes de configuration, quitter nano en enregistrant ces modifications)
sudo shutdown -r now  (ou sudo reboot)
```

Le fichier KlipperScreen.conf :
```
[main]

[printer Nom-de-l-imprimante-a-afficher]
moonraker_host: IP-de-l-imprimante
moonraker_port: 7125
```

Ce qui donne par exemple chez moi :
```
[main]

[printer X-Max 3]
moonraker_host: 192.168.1.126
moonraker_port: 7125
```

A l'issue du redémarrage, si tout s'est bien passé, l'écran de Klipperscreen devrait s'afficher

## Démontage / Montage

C'est la partie la moins amusante. L'ancien écran sur la X-Max3 est maintenu par quatre vis sur la façade avant. Seules les deux au bas de l'écran sont accessibles 😞 même après avoir retiré le plastique autocollant. 

<p align="center">
<img src="/Images/acces-ecran-autocollant-retire.png"> <img src="/Images/vis-ecran.png">
</p>

Pour pouvoir accéder aux deux dernières vis, il m'a fallu, retirer les deux coques de côté, puis la façade avant (je n'ai pas repris de photos à cette occasion, celle ci-dessous est issue du test de cette imprimante pour le forum) 

<p align="center">
<img src="/Images/xmax3-a-coeur-ouvert.jpg">
</p>

Une fois l'écran débranché, les quatre vis ôtées, on peut l'enlever

<p align="center">
<img src="/Images/qidi-ecran-1.png"> <img src="/Images/qidi-ecran-2.png">
</p>

Le passage des câbles (HDMI, USB) est facilité avec les coques démontées. Un adaptateur imprimé permet de monter le nouvel écran dans l'ancien emplacement (à noter qu'avec cet adaptateur, l'écran rentre au chausse-pieds, une barre métallique sur la X-max 3 met en contrainte l'arrière de l'écran donc la réinstallation de la façade avant doit être réalisée délicatement 😉 ).

Test pour vérifier que tout fonctionne avant de remettre les coques de côté et la façade avant, et l'autocollant plastique.

Au final, l'imprimante est désormais totalement open source 😄 

<p align="center">
<img src="/Images/ks-accueil.png"> <img src="/Images/ks-accueil-plus.png">
</p>

A noter que l'alimentation du RPi0V2 est comme celle de l'imprimante sur une prise électrique pilotable (Tasmota + Home Assistant). L'extinction de l'imprimante est réalisée par des macros Gcode, plus des macros Shell command qui permettent d'exécuter des scripts shell (le RPi0v2 est d'abord arrêté proprement (shutdown) avant de couper son alimentation électrique). A l'allumage de l'imprimante, l'écran est lui aussi automatiquement allumé (le «delayed_gcode klipperscreen_on» s'en charge) :
- macros
```
[gcode_shell_command klipperscreen_halt]
 command: bash -c "bash $HOME/printer_data/config/scripts/halt_klipperscreen.sh"
 timeout: 30
 verbose: True

[gcode_macro KS_LAN_HALT]
 description: Remote Shutdown Klipperscreen
 gcode:
     RUN_SHELL_COMMAND CMD=klipperscreen_halt

#=====================================================
# Power Operations / HA Plug
#=====================================================
[gcode_macro POWER_ON_KS]
gcode:
  {action_call_remote_method("set_device_power",
                             device="Klipperscreen",
                             state="on")}
							 
[gcode_macro POWER_OFF_KS]
gcode:
  {action_call_remote_method("set_device_power",
                             device="Klipperscreen",
                             state="off")}
  
[gcode_macro POWER_OFF_PRINTER]
gcode:
  {action_call_remote_method("set_device_power",
                             device="Qidi_XMax3",
                             state="off")}
  
[gcode_macro POWER_OFF_ALL]
gcode:
  KS_LAN_HALT
  G4 P20000 # wait 20 seconds to shutdown RPi0V2
  POWER_OFF_KS
  # Find a way, if ever possible to shutdown filesystem before power off
  POWER_OFF_PRINTER

[delayed_gcode delayed_printer_off]
initial_duration: 18000.
gcode:
  {% if printer.extruder.target >= 50 or printer.extruder.temperature >= 50 or printer.heater_bed.temperature >= 50 or printer.idle_timeout.state == "Printing" %}
    UPDATE_DELAYED_GCODE ID=delayed_printer_off DURATION=300    
  {% else %}
    POWER_OFF_ALL
  {% endif %} 
    
[delayed_gcode klipperscreen_on]
initial_duration: 1.
gcode:
  POWER_ON_KS
```
- script (halt_klipperscreen.sh) => nécessite l'installation du paquet sshpass (`sudo apt install -y sshpass`):
```
#!/bin/bash

# Klipperscreen IP address
IP_KS="192.168.1.127"

# Username and password
USER="mks"
PASSWORD="makerbase"

# Define countdown function
countdown() {
    for i in {5..1}; do
        echo "Shutting down Klipperscreen in: $i"
        sleep 1
    done
    echo "Shutting down Klipperscreen in: 0"
}

# Display countdown
echo "Klipperscreen will be shut down:"
countdown

# Establish SSH connection with disabled host key checking and shut down Klipper screen
sshpass -p "$PASSWORD" ssh -o StrictHostKeyChecking=no $USER@$IP_KS "sudo shutdown -h now"

# Check if the Klipper screen has been shut down
sshpass -p "$PASSWORD" ssh -o StrictHostKeyChecking=no $USER@$IP_KS "pgrep klipperscreen > /dev/null"
if [ $? -eq 0 ]; then
  echo "Error: Klipperscreen was not shut down!"
  exit 1
fi

echo "Shutdown completed!"
```
- moonraker.conf
```
# Gestion prises électriques via Tasmota
[power Qidi_XMax3]
# athom-sp2
type: tasmota
address: 192.168.1.186

[power Klipperscreen]
# athom-sp1
type: tasmota
address: 192.168.1.185
```
 😃
