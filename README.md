# Zigate-Legrand-with-Netatmo-HA
Partage de la configuration pour faire fonctionner les interrupteurs Legrand with Netatmo (Celiane with Netatmo, Mosaïc with Netatmo, Dooxie with Netatmo etc...) sur Home Assistant avec une Zigate en utilisant le plugin de doudz.

Lien vers le plugin de doudz pour la Zigate sur Home Assistant :
https://github.com/doudz/homeassistant-zigate

Pré-requis :
- Avoir un serveur Home Assitant fonctionnel
- Avoir installé une Zigate (ou Pizigate etc..) sur ce serveur
- Avoir installé le plugin de doudz sur ce serveur pour commander la Zigate
- Avoir appairé l'interrupteur filaire Legrand avec votre serveur via la Zigate

_____________________________________________________________________________
Configuration avec variateur d'intensité :

Pour activer l'option variateur sur un interrupteur Legrand with Netatmo filaire, il faut : appeller le service "zigate.raw_command" avec les données suivantes :

cmd: 0x0530

data: '02 abcd 01 01 fc01 0104 00 00 08 00 01 02 0000 09 0101 f1'

en changeant "abcd" par l'attribut "addr" de l'entité de l'interrutpeur dans Home Assistant (à trouver dans l'onglet "Outils de développement" > "état"

Depuis une MAJ du plugin de doudz, il n'y a plus besoin de modifier le fichier "customize.yaml" pour gérer correctement les interrupteurs Legrand with Netatmo
________________________________________________________________________________
Configuration sans variateur d'intensité :

Il n'y a pas besoin de modifier le fichier "customize.yaml"
Pour désactiver l'option variateur sur un interrupteur Legrand with Netatmo filaire, il faut : appeller le service "zigate.raw_command" avec les données suivantes :

cmd: 0x0530

data: '02 abcd 01 01 fc01 0104 00 00 08 00 01 02 0000 09 0100 f1'

en changeant "abcd" par l'attribut "addr" de l'entité de l'interrutpeur dans Home Assistant (à trouver dans l'onglet "Outils de développement" > "état"

_____________________________________________________________________________
Appairage des interrupteurs Legrand with Netatmo sans fil avec des interrupteur Legrand with Netatmo filaire :

L'objectif est de contrôler les interrupteurs filaires avec les sans fil, de la même façon qu'avec la passerrelle Legrand "de base". Ainsi ces interrupteurs sans fil peuvent allumer / eteindre / faire varier l'intensité des interrupteurs filaires. De plus, avec un apparaige direct, les 2 interrupteurs communiquent directement : en cas de panne de la passerelle / Zigate, les communications ne sont pas interrompues.

Il faut d'abord que les interrupteurs sans fils que l'on souhaite utiliser soient déjà appairée avec la zigate (comme n'importe quel autre appareil zigbee), de même que l'interrupteurs filaires.
Il faut récupérer leur IEEE : que l'on peut trouver dans home assistant dans les outils de développement ou dans la page zigate admin.

Si on utilise une Zigate-USB, il faut connecter la zigate sur un port USB d'un PC sous Windows.

Si on utilise une PiZigate, il faut la connecter via un adaptateur USB-TTL sur un port USB d'un PC sous Windows. Pour cela il faut connecter des cables entre l'adaptateur USB-TTL et la PiZigate : il faut relier le GND (ground) de l'adaptateur au GND de la PiZigate, le RX de l'adaptateur au TX de la PiZigate et le TX de l'adaptateur au RX de la PiZigate.
On peut s'aider de ce diagramme du port de la PiZigate pour repérer les branchements (ATTENTION LA VU DE CE DIAGRAMME EST "PAR DESSUS" DONC LORSQU'ON BRANCHE LES CABLES, ON EST DESSOUS LE CONNECTEUR DE LA PIZIGATE ET IL FAUT DONC "INVERSER" DE DROITE A GAUCHE / FAIRE UNE SYMETRIE HORIZONTALE DU CONNECTEUR, PAR EXEMPLE VCC SE RETROUVE A DROITE DU CONNECTEUR VU DE DESSOUS) :
https://i0.wp.com/zigate.fr/wp-content/uploads/2019/03/connecteur.png?w=507&ssl=1

Dans le gestionnaire de périphériques il faut repérer quel est le port COM associé à la zigate (ex COM3).
Il faut télécharger ce programme : https://github.com/fairecasoimeme/ZiGate/raw/master/Tools/TestGUI_ZiGate_NXP-4.zip
Puis l'extraire et lancer "ZGWUI".
https://user-images.githubusercontent.com/36080343/79689220-9f9d3d00-8253-11ea-9f9a-8f642bf64ebc.png

Cliquer sur "Settings"
https://user-images.githubusercontent.com/36080343/79689235-b2177680-8253-11ea-9f92-a6719d5409b2.png

Puis sélectionner dans la liste le bon port COM (il n'y en aura probablement qu'un, avec le "Baud rate à 115200 puis ok.
Puis cliquer sur "open port"
https://user-images.githubusercontent.com/36080343/79689270-d7a48000-8253-11ea-833a-b6e2752b02b8.png

Cliquer ensuite sur "Get Version".
Puis paramétrer le canal 11 et cliquez sur « Set CMSK », choisir « COORDINATOR » puis cliquez sur « Set Type » puis cliquez sur « Start NWK »
https://user-images.githubusercontent.com/36080343/79689294-10dcf000-8254-11ea-83c6-5ce75978773b.png

Maintenant la communication directe avec la zigate est établie.

Pour faire l'association d'un interrupteur sans fils sur un filaire également déja appairé à la zigate :
- s'assurer que l'inter sans fil n'est pas en veille : appuyer sur le bouton off par exemple.
- dans les 5 secondes il faut faire le bind des cluster 6 puis 8 de la télécommande vers l'interrupteur filaire : remplir comme ci dessous, en 1 l'IEEE de l'inter legrand with netatmo sans fil, en 2 l'IEEE de l'interrupteur filaire, en 3 le cluster (08 et 06) en veillant à ne pas mettre les "0x" ou les premier "00" des IEEE, et en mettant dans le menu déroulant "IEEE" puis en cliquant sur le bouton "Bind"
https://user-images.githubusercontent.com/36080343/79689353-88128400-8254-11ea-9462-bce2cfb4d7d1.png

