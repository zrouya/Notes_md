---
tags: [index, docker]
---

# Docker — Map of Content

## Fondamentaux

- [[docker]] — vue d'ensemble, workflow build/ship/run
- [[installation-de-docker]] — Docker Engine, CLI, Docker Desktop, WSL
- [[docker-daemon]] — le daemon `dockerd`
- [[commandes-docker]] — structure des commandes directes / de management
- [[option-format]] — personnaliser l'affichage des commandes avec `--format`
- [[orchestrateur-de-conteneurs]] — rôle générique d'un orchestrateur

## Images & Build

- [[images-docker]] — composition d'une image Docker
- [[docker-files]] — le Dockerfile et ses stanzas principales
- [[build-des-images-docker]] — `docker image build`
- [[build-multi-stage-images-docker]] — optimiser la construction en plusieurs stages
- [[layers-images-docker]] — empilement et cache des layers
- [[copy-and-write]] — copy-on-write entre image et conteneur
- [[ufs-layer]] — layer en lecture-écriture du conteneur
- [[standard-oci]] — standard OCI issu des images Docker

## Registry & Distribution

- [[docker-registry]] — stockage et distribution des images
- [[docker-repository]] — notion de repository
- [[dockerhub]] — registry par défaut de Docker
- [[tags-des-images-docker]] — tags et versionnement des images

## Conteneurs

- [[containers-docker]] — isolation par namespaces et cgroups
- [[execution-des-conteneurs-docker]] — `docker container run` et gestion des containers
- [[management-des-conteneurs-docker]] — monitorer et interagir avec des conteneurs
- [[docker-healthchecks]] — vérifier l'état de santé des conteneurs

## Volumes & Stockage

- [[volumes-docker]] — volumes gérés par Docker
- [[bind-mounts-docker]] — montage d'un emplacement de l'hôte
- [[tmpfs-mounts-docker]] — montage en mémoire
- [[gestion-des-donnees-docker]] — espace disque et nettoyage (`prune`)

## Docker Compose

- [[docker-compose]] — build, déploiement et run d'ensembles de conteneurs
- [[fichier-docker-compose]] — structure du fichier yaml
- [[docker-compose-build]] — paramètres de build dans un service
- [[docker-compose-cli]] — outil CLI `docker compose`
- [[extensions-de-docker-compose-files]] — factoriser des configurations communes

## Docker Swarm

- [[swarm/docker-swarm]] — orchestrateur intégré à Docker
- [[swarm/initialisation-de-docker-swarm]] — `docker swarm init`
- [[swarm/noeuds-swarm]] — nœuds du cluster
- [[swarm/manager-swarm]] — gestion du cluster et consensus Raft
- [[swarm/workers-swarm]] — exécution des tâches
- [[swarm/services-docker-swarm]] — abstraction de service
- [[swarm/tasks-docker-swarm]] — instances d'un service
- [[swarm/mise-a-jour-de-services-docker-swarm]] — mise à jour progressive et rebalancing
- [[swarm/docker-swarm-secrets]] — gestion des secrets
- [[swarm/docker-swarm-stacks]] — déploiement via fichier yaml de stack
- [[swarm/docker-swarm-routing-mesh]] — routage réseau entre nœuds
