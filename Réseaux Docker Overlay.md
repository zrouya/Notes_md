
Overlay est un **type de [[Drivers réseau Docker|driver]]** réseau fourni par [[Docker]], permettant de faire communiquer plusieurs instances de [[Docker Daemon|deamons Docker]] entre elles. Il est notamment souvent utilisé pour faire communiquer des [[Noeuds Swarm|noeuds]] ensemble, au sein d'un [[Docker Swarm|swarm]] Docker.

La création d'un réseau Overlay se fait en spécifiant le driver correspondant : ``docker network create --driver overlay myNetwork``.

Note : Il est possible de configurer un réseau Overlay pour utiliser le protocole [[Protocole IPSec|IPSec]] (**chiffrement et authentification au sein d'un protocole IP**), mais cette option est désactivée par défaut pour des raisons de performance.

Lors de la **création** d'un [[Services Docker Swarm|service]] Swarm, il suffit de **spécifier le nom du réseau** pour que le service y soit **accessible (via son nom de service)** : ``docker service create --network myNetwork -p 80:80 newService``.

Il est possible de spécifier **plusieurs réseaux** : ``docker service create --network net1 --network net2 -p 80:80 new Service``.