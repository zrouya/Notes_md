---
tags: [docker, compose]
---

# Docker Compose

[[docker|Docker]] Compose est un outil permettant de gérer le build, le déploiement, et l'**exécution d'ensembles de conteneurs**, généralement voués à communiquer ensemble. Il permet de **réutiliser** les différents **paramètres**, notamment d'exécution des conteneurs, en les stockant dans des fichiers faciles à utiliser.

## Composition

Docker Compose est constitué de 2 parties :

- Un [[fichier-docker-compose|fichier yaml]] qui décrit les différentes options de build ou de run : des [[containers-docker|containers]], des [[Gestion du réseau Docker|networks]], des [[volumes-docker|volumes]], etc.
- Un [[docker-compose-cli|outil CLI]] ``docker-compose`` qui consomme le(s) fichier(s) yaml, et qui effectue les actions correspondantes avec les options qui y sont décrites, via la commande ``docker compose up`` (à effectuer dans le répertoire du fichier yaml).

Documentation : voir https://docs.docker.com/compose/compose-file/

Note : Docker Compose est un **outil très pratique** pour les environnements de **développement**, de **test**, ou de petits environnements de production (ou de production isolée), mais pour des projets plus complexes en production, il est préférable de se reposer sur un orchestrateur plus puissant, comme [[docker-swarm|Docker Swarm]] ou [[Kubernetes]] (même si une utilisation collaborative est possible, voir https://github.com/BretFisher/ama/discussions/146).

## Voir aussi

- [[fichier-docker-compose]]
- [[docker-compose-cli]]
- [[docker-compose-build]]
- [[extensions-de-docker-compose-files]]
- [[docker-swarm-stacks]]
