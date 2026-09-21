
La gestion réseau de l'environnement Docker est basée sur l'approche **"Batteries included, but removable"**. La configuration par défaut fonctionne dans la plupart des cas, mais il est facile de l'adapter.

Les conteneurs sont reliés à des **réseaux privés virtuels** sur l'hôte :
- Un conteneur peut être connecté à un, plusieurs, ou aucun de ces réseaux virtuels
- Par défaut un conteneur est relié au **VPN "bridge"** ou **"docker0"**
- Il est possible de créer et manager ces VPN
- Un conteneur peut accéder au réseau de l'hôte ou à internet via un **firewall NAT**
- Un conteneur peut "bypasser" ces réseaux virtuels et **utiliser l'IP de l'hôte** (``--net=host``)
- En configuration par défaut, un conteneur **n'a pas la possibilité de communiquer** avec un conteneur relié **à un autre VPN**, sauf en passant par les **ports publiés**, via le **réseau hôte**.

Il est possible d'utiliser des [[Drivers réseau Docker|drivers réseau]] Docker additionnels pour enrichir les fonctionnalités et possibilités de configuration.

Le schéma suivant résume le cas de 2 conteneurs, sur 2 bridges différents :
![[Pasted image 20240207141758.png]]

**Note :** Les conteneurs ne se servent pas des adresses IP pour communiquer entre eux au sein d'un même VPN (car ces adresses sont dynamiques, varient en fonction du cycle de vie des conteneurs).
Tout **network custom** est créé nativement avec un **[[Domain Name System (DNS)]]** qui identifie, par défaut, les conteneurs **[[DNS Docker|par leur nom]]**.
Le VPN par défaut (bridge ou docker0) n'**implémente pas de DNS**, il est donc recommandé de **toujours créer un VPN custom**.
### Commandes principales pour gérer l'aspect réseau :

- **Option --publish (-p)** : Mappe un port de l'hôte sur un port du container : 

``docker container run [Image] -p [port_host]:[port_container] 

- ``docker container port [Container]`` permet de voir le mappage défini par la commande précédente (option --publish)

- La commande ``docker container inspect [Container]`` retourne un json qui contient entre autres des informations réseau sur le conteneur ([[Option --format|formatter]] l'output pour extraire les informations. ex : ``docker container inspect --format {{.NetworkSettings.IPAddress}} myContainer``).

- Pour les bridges VPN : 
	- ``docker network ls``
	- ``docker network inspect``
	- ``docker network create --driver``
	- ``docker network connect [Container] [Network]`` : ajoute une interface ([[Carte d'interface réseau|NIC]] virtuelle) à un conteneur pour accéder au VPN spécifié
	- ``docker network disconnect [Container] [Network]`` : supprime l'interface entre le conteneur et le VPN

