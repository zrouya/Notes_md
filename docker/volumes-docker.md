---
tags: [docker, volumes, stockage]
---

# Volumes Docker

Un volume Docker est un **emplacement spécial** dans le système de fichiers, dont le contenu **n'est pas supprimé lors de la suppression du conteneur**. Un volume Docker doit être [[gestion-des-donnees-docker|supprimé manuellement]].

## Création

On définit un volume Docker par une stanza ``VOLUME`` dans le Dockerfile :

```docker
VOLUME /var/lib/mysql
```

Cette commande va créer un [[bind-mounts-linux|bind mount]] Linux, un mappage entre le répertoire du conteneur (ici, var/lib/mysql) et un répertoire de l'hôte. Les volumes Docker sont stockés, au sein de l'hôte, dans un emplacement **géré par Docker** (sous Linux, **var/lib/docker/volumes**). **Un process hors Docker ne devrait pas modifier cet emplacement du système de fichier**. Pour cela, il convient d'utiliser plutôt un [[bind-mounts-docker|bind mount Docker]].

Après l'exécution du conteneur en question, ce nouveau volume pourra être retrouvé par la commande ``docker volume ls``.

**Note :** l'option ``--volume (-v)`` de la commande ``docker run`` permet d'obtenir le même résultat : ``docker container run -d --name mysqlcontainer -v /var/lib/mysql mysql``

## Volumes nommés

Dans la mesure où les volumes Docker persistent au-delà des conteneurs (même quand ceux-ci sont supprimés), il est intéressant d'attribuer un nom à ces volumes. Pour cela, il suffit de préfixer l'option volume ci-dessus :

``docker container run -d --name mysqlnamedvolume -v volumeName:/var/lib/mysql mysql``

## Voir aussi

- [[bind-mounts-docker]]
- [[tmpfs-mounts-docker]]
- [[gestion-des-donnees-docker]]
