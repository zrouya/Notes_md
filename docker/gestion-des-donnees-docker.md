---
tags: [docker, volumes, stockage]
---

# Gestion des données Docker

Il est possible de consulter l'ensemble de l'espace disque occupé par les images, conteneurs, volumes, et build cache [[docker-overview|Docker]] en exécutant la commande ``docker system df``.

## Libérer de l'espace disque

Pour libérer de l'espace disque, utiliser la commande 'prune' :
- ``docker system prune`` pour l'ensemble des éléments non utilisés
- ``docker image prune``
- ``docker container prune``
- ``docker volume prune``

## Voir aussi

- [[volumes-docker]]
- [[bind-mounts-docker]]
- [[tmpfs-mounts-docker]]
