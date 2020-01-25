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

L'objectif est de contrôler les interrupteurs filaires avec les sans fil, de la même façon qu'avec la passerrelle Legradn "de base". Ainsi ces interrupteurs sans fil peuvent allumer / eteindre / faire varier l'intensité des interrupteurs filaires. De plus, avec un apparaige direct, les 2 interrupteurs communiquent directement : en cas de panne de la passerelle / Zigate, les communications ne sont pas interrompues.
Appairage :

en construction
