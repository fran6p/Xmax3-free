# ÉCRAN TACTILE

## Écran originel

L'écran installé sur l'imprimante est un écran type HMI (Human Machine Interface), **en français IHM (interface Homme Machine)**, plusieurs fabricants en proposent, Nextion et sa variante asiatique TJC sont les plus connus.

Ces écrans HMI sont des ordinateurs monocartes à part entière, fonctionnant avec leur propre micrologiciel (firmware).  Ils ne sont pas spécifiques aux imprimantes Qidi, mais le micrologiciel qui les fait fonctionner a été programmé spécifiquement pour chaque modèle.

Ce type d'écran communique avec la carte contrôleur en échangeant des informations avec celle-ci par le biais d'événements.  Ce n'est donc pas comme un écran HDMI qui lui affiche une image en fonction du signal envoyé.  
Il s'agit plutôt d'une transmission de messages, par exemple :
- la carte contrôleur envoie un message X à l'écran, X étant l'écran de chargement du filament ou
- l'écran envoie un message à la carte contrôleur que le bouton 007 a été enfoncé, ce bouton 007 correspondant au bouton pour extruder le filament de 20 mm.

Pour que la communication se fasse entre carte et écran, un logiciel est utilisé: Xindi

Ce programme d'interface d'écran de Qidi agit comme un serveur convertissant les messages de type IHM (Interface Homme Machine) en commandes à envoyer à Moonraker/Klipper, et vice-versa.

La communication entre l'écran et la carte est de type série ( [UART](https://fr.wikipedia.org/wiki/UART) ), son débit de données est lent, à 115200 bauds, soit 115200 bits/s (≃ 14400 octets/s (8N1, 8 bits, pas de parité, 1 bit de stop) => c'est pourquoi les mises à jour du firmware de l'écran sont si lentes.  Le micrologiciel de l'écran a une taille d'environ 20 Mo, et à chaque mise à jour du firmware de l'écran, la carte de contrôle doit envoyer 20 Mo sur une liaison de 14,4 Ko/s, ce qui prend une bonne demi-heure (*on peut aller plus vite en utilisant une carte μSD sur laquelle a été copié le firmware écran introduite dans le lecteur SD à l'arrière de l'écran*). Les autres mises à jour de Qidi sur la carte contrôleur principale sont effectuées en moins de 5 secondes.

