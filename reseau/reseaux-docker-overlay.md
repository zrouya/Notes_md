---
tags: [docker, reseau]
---

# Réseaux Docker Overlay

Overlay est un **type de [[drivers-reseau-docker|driver]]** réseau fourni par [[docker-overview]], permettant de faire communiquer plusieurs instances de [[docker-daemon|deamons Docker]] entre elles. Il est notamment souvent utilisé pour faire communiquer des [[noeuds-swarm|noeuds]] ensemble, au sein d'un [[docker-swarm|swarm]] Docker.

## Création et usage

La création d'un réseau Overlay se fait en spécifiant le driver correspondant : ``docker network create --driver overlay myNetwork``.

Note : Il est possible de configurer un réseau Overlay pour utiliser le protocole [[Protocole IPSec|IPSec]] (**chiffrement et authentification au sein d'un protocole IP**), mais cette option est désactivée par défaut pour des raisons de performance.

Lors de la **création** d'un [[services-docker-swarm|service]] Swarm, il suffit de **spécifier le nom du réseau** pour que le service y soit **accessible (via son nom de service)** : ``docker service create --network myNetwork -p 80:80 newService``.

Il est possible de spécifier **plusieurs réseaux** : ``docker service create --network net1 --network net2 -p 80:80 newService``.

## Voir aussi

- [[drivers-reseau-docker]]
- [[gestion-reseau-docker]]
