

[[Docker]] Compose est un outil permettant de gérer le build, le déploiement, et l'**exécution d'ensembles de conteneurs**, généralement voués à communiquer ensemble.

Il permet de **réutiliser** les différents **paramètres**, notamment d'exécution des conteneurs, en les stockant dans des fichiers faciles à utiliser.

Docker compose est constitué de 2 parties :

- Un [[Fichier Docker compose|fichier yaml]] qui décrit les différentes options de build ou de run : 
	- des [[Containers Docker|containers]]
	- des [[Gestion du réseau Docker|networks]]
	- des [[Volumes Docker|volumes]]
	- ....
- Un [[Docker Compose CLI|outil CLI]] ``docker-compose`` qui consomme le(s) fichier(s) yaml, et qui effectue les actions correspondantes avec les options qui y sont décrites, via la commande ``docker compose up`` (à effectuer dans le répertoire du fichier yaml).

Documentation : voir https://docs.docker.com/compose/compose-file/

Note : Docker Compose est un **outil très pratique** pour les environnements de **développement**, de **test**, ou de petits environnements de production (ou de production isolée), mais pour des projets plus complexe en production, il est préférable de se reposer sur un orchestrateur plus puissant, comme [[Docker Swarm]] ou [[Kubernetes]] (même si une utilisation collaborative est possible. voir https://github.com/BretFisher/ama/discussions/146).