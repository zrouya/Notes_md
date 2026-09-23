
Dans Docker, chaque **réseau custom** créé (``docker network create``), implémente **son propre serveur DNS**, qui associe **chaque conteneur** à son **nom en tant qu'alias** (à chaque fois qu'un conteneur est associé à un VPN, le DNS correspondant est mis à jour).

Ces alias peuvent être modifiés avec l'option **--network-alias** : 
``docker container run --network (ou --net) mynetwork --network-alias newalias``


Pour assurer la **haute disponibilité** et le **load balancing**, il est possible d'attribuer le **même alias à plusieurs conteneurs**, de sorte qu'une requête entrante peut être traitée par n'importe lequel de ces conteneurs redondants.

Par défaut, le DNS routera la requête au hasard vers un des conteneurs répondant au même alias ([[Round Robin DNS]]).

Note : 
Pour vérifier quel conteneur est appelé à chaque requête, il est possible d'effectuer des appels curl (intégré à la distribution centos, par exemple) : 
``docker container run --network mynetwork --rm centos curl -s newalias:port``
