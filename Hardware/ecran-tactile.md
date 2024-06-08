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

Comme on l'a vu ci-dessus, pour fonctionner, le firmware de l'écran originel doit pouvoir communiquer avec Moonraker / Klipper par l'intermédiaire d'un programme (Xindi). En installant une image du systèmè plus récente (Armbian Bookworm + écosystème Klipper «vanilla»), ce programme n'a pas été réinstallé.

Pourquoi ?

Xindi et le microgiciel de l'écran utilisent des méthodes / événements codés en «dur» (certains chemins par exemple sont différents (configuration de Klipper, journaux, Gcodes) entre l'écosystème Klipper «officile» et celui de Qidi), le Zoffset est géré par l'écran, sa valeur est stockée dans le fichier ***config.mksini***, des fichiers Python de Klipper et Moonraker ont été modifiés pour pouvoir gérer cet écran.

Donc, le simple ajout de Xindi ne permettrait pas le fonctionnement correct de l'écran.

Au premier allumage de l'imprimante après installation du nouveau système, le firmware autonome de l'écran ne pouvant communiquer avec la carte, provoque cet affichage sur l'écran
 
<p align="center">
<img src="/Images/BSOD-screen.jpg">
</p>


