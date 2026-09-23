---
tags: [docker, installation]
---

# Installation de Docker

L'installation de [[docker-overview|Docker]] diffère selon la destination souhaitée (OS, local ou serveur, ...). Docker Engine est le binaire qui permet l'exécution des conteneurs (``dockerd``, ou Docker daemon) et nécessite un kernel Linux ; Docker CLI permet d'envoyer des commandes Docker à l'instance de Docker Engine voulue.

## Installation locale

Pour une installation de Docker en local, Docker Desktop permet d'installer simplement Docker Engine, CLI, et d'autres features (compatible Windows, Linux).

Sur Windows (10+), il est possible de configurer une machine virtuelle Linux (hébergeant une distribution Linux particulière) grâce au Windows Subsystem for Linux (WSL). Docker Desktop embarque également une intégration du WSL, pour permettre d'installer Docker Engine sur cette distribution.

## Voir aussi

- [[docker-overview]]
- [[docker-daemon]]
