---
tags: [docker, cli]
---

# Commandes Docker

Les commandes Docker sont réparties en deux sortes : les commandes « directes » (``docker <command>``) et les commandes « de management », organisées en sous-commandes (``docker <command> <sub-command>``). Certaines commandes sont exécutables des deux manières, pour assurer la **rétrocompatibilité** des différentes versions de Docker.

## Exemples

- Commande directe : ``docker run --detach --publish 80:80 nginx``
- Commande de management : ``docker container rm myContainerName``

Doc Docker sur les commandes de base : https://docs.docker.com/engine/reference/commandline/docker/

## Voir aussi

- [[execution-des-conteneurs-docker]]
- [[management-des-conteneurs-docker]]
- [[option-format]]
