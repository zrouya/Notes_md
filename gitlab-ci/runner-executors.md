---
tags: [gitlab-ci, runner, docker, executor]
---

# GitLab Runner — Executors

Un executor définit *comment* le runner exécute les commandes d'un job CI.

## Shell executor

Exécute les commandes directement sur la machine hôte, sans container.

- L'image `image:` définie dans le pipeline est **ignorée**
- Docker est disponible si installé sur l'hôte
- Pas d'isolation entre jobs

## Docker executor

Démarre un container pour chaque job en utilisant l'image définie dans `image:`.

- Bonne isolation, environnement reproductible
- `docker` n'est **pas disponible** dans le container par défaut
- Nécessite le [[docker-socket-monte]] pour accéder au daemon hôte

## Identifier son executor

Dans `config.toml` du runner :
```toml
[[runners]]
  executor = "shell"       # ou "docker"
```

Ou tester en lançant un job : si `docker pull` échoue avec
`Cannot connect to the Docker daemon`, c'est un Docker executor sans socket monté.

## Voir aussi

- [[docker-socket-monte]]
- [[gitlab-ci-pull-policy]]
