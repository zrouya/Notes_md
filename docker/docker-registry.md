---
tags: [docker, registry]
---

# Docker registry

Un registry Docker est une **application serveur** qui **stocke et distribue** un référentiel d'[[standard-oci|images OCI]]. Les images sont définies par un nom (path) au sein d'un registry.

La récupération d'une image se fait via la commande ``docker image pull``, et la mise à jour ou l'ajout d'une image via la commande ``docker image push`` (pour cela, il faut être loggé, via la commande ``docker login``). Le registry utilisé par défaut par Docker est [[dockerhub|DockerHUB]], mais il est possible d'utiliser d'autres registry, via AWS, Azure, Google Cloud, ou un registry déployé.

## Voir aussi

- [[dockerhub]]
- [[docker-repository]]
- [[standard-oci]]
