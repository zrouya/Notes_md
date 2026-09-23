---
tags: [docker, compose, cli]
---

# Docker Compose CLI

L'outil Docker Compose CLI permet de définir et de gérer des applications multi-conteneurs avec Docker (il génère des commandes Docker CLI qui sont envoyées au Docker daemon). Il est intégré par défaut dans les installations Docker pour Windows ou Mac ; pour les installations Linux, cet outil est à installer à part (voir https://github.com/docker/compose/releases).

## Commandes principales

- ``docker compose up`` : configure les volumes/réseaux, et lance les conteneurs
- ``docker compose down`` : arrête les containers et les supprime, ainsi que les volumes et réseaux Docker

Le reste des commandes principales est inspiré des commandes Docker classiques :
- ``docker compose up -d`` permet d'exécuter les conteneurs en mode détaché (comme pour ``docker container run -d``). Par défaut, la console affichera les logs des différents conteneurs, avec un code couleur aléatoire pour différencier les conteneurs.
- ``docker compose logs`` agrège les logs des différents conteneurs.
- ``docker compose ps`` liste les conteneurs en exécution.
- ``docker compose top`` liste les processus Docker en exécution sur l'hôte.

## Voir aussi

- [[docker-compose]]
- [[fichier-docker-compose]]
- [[docker-compose-build]]
