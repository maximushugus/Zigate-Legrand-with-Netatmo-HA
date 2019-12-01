# Zigate-Legrand-with-Netatmo-HA
Partage de la configuration pour faire fonctionner les interrupteurs Legrand with Netatmo (Celiane with Netatmo, Mosaïc with Netatmo, Dooxie with Netatmo etc...) sur Home Assistant avec une Zigate en utilisant le plugin de doudz.

Lien vers le plugin de doudz pour la Zigate sur Home Assistant :
https://github.com/doudz/homeassistant-zigate

Pré-requis :
- Avoir un serveur Home Assitant fonctionnel
- Avoir installé une Zigate (ou Pizigate etc..) sur ce serveur
- Avoir installé le plugin de doudz sur ce serveur pour commander la Zigate
- Avoir appairé l'interrupteur filaire Legrand avec votre serveur via la Zigate

Le fichier "Interrupteurs filaires avec variateur.yaml" correspond la la configuration type d'une lumière contrôlant un interrupteur filaire avec l'option variateur activée. Cette configuration est à ajouter aux entités des lumières dans le fichier "customize.yaml" du serveur Home Assistant.
Pour activer l'option variateur sur un interrupteur Legrand with Netatmo filaire, il faut : appeller le service "zigate.raw_command" avec les données suivantes :

cmd: 0x0530

data: '02 abcd 01 01 fc01 0104 00 00 08 00 01 02 0000 09 0101 f1'

en changeant "abcd" par l'attribut "addr" de l'entité de l'interrutpeur dans Home Assistant (à trouver dans l'onglet "Outils de développement" > "état"

Le fichier "Interrupteurs filaires avec variateur.yaml" correspond la la configuration type d'une lumière contrôlant un interrupteur filaire avec l'option variateur désactivée. Cette configuration est à ajouter aux entités des lumières dans le fichier "customize.yaml" du serveur Home Assistant.
Pour désactiver l'option variateur sur un interrupteur Legrand with Netatmo filaire, il faut : appeller le service "zigate.raw_command" avec les données suivantes :

cmd: 0x0530

data: '02 abcd 01 01 fc01 0104 00 00 08 00 01 02 0000 09 0100 f1'

en changeant "abcd" par l'attribut "addr" de l'entité de l'interrutpeur dans Home Assistant (à trouver dans l'onglet "Outils de développement" > "état"
