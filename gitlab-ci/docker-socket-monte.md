---
tags: [gitlab-ci, docker, runner, socket]
---

# Docker socket monté dans un container CI

Permet à un container GitLab CI d'accéder au daemon Docker de l'hôte,
et donc d'exécuter des commandes `docker` depuis l'intérieur du job.

## Principe

Le daemon Docker écoute sur `/var/run/docker.sock`.
En montant ce fichier dans le container, les commandes `docker` y parlent
au daemon de l'hôte (pas à un daemon isolé).

## Configuration (config.toml du runner)

```toml
[[runners.docker]]
  volumes = ["/var/run/docker.sock:/var/run/docker.sock"]
```

## Cas d'usage

- `docker pull` dans un `before_script` pour forcer le rechargement d'une image taguée `latest` ou `dev`
- `docker build` dans un job sans DinD

## Risque

Le container a accès complet au daemon hôte → accès à tous les containers
et images de la machine. À réserver aux runners de confiance.

## Alternative : Docker-in-Docker (DinD)

Lance un daemon Docker isolé comme service du job.
Plus sûr, mais plus lourd à configurer.

```yaml
services:
  - name: docker:24-dind
    alias: docker
variables:
  DOCKER_HOST: tcp://docker:2375
```

## Voir aussi

- [[runner-executors]]
- [[gitlab-ci-pull-policy]]
