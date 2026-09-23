---
tags: [docker, volumes, stockage]
---

# Bind mounts Docker

Un bind mount Docker fonctionne comme un [[volumes-docker|volume]] Docker, mais il permet de connecter un conteneur à un emplacement du système de fichiers de l'hôte **qui n'est pas managé par Docker**. Ceci permet à un conteneur d'avoir accès, pour persister les données, à un emplacement de l'hôte **accessible par des processus tiers**.

## Création

Dans la mesure où un bind mount n'est pas managé par Docker (en termes de file system), sa création **ne peut pas faire partie du Dockerfile**, mais **doit être spécifiée au démarrage** du conteneur :

```docker
docker container run -d -p 80:80 --name nginx \
--mount target=bind, source=/cmp, target=/var/app
```

Note : il est également possible de spécifier un bind mount avec l'option ``--volume`` ou ``-v``, mais cela est **déconseillé** pour des raisons de **lisibilité**, même si l'option ``--mount`` est plus verbeuse.

Note : il est possible d'utiliser le raccourci Linux ``$(pwd)`` (**print working directory**) :
``docker container run -d --mount type=bind, source="$(pwd)"/source/dir, target=/app``

## Voir aussi

- [[volumes-docker]]
- [[tmpfs-mounts-docker]]
- [[gestion-des-donnees-docker]]
