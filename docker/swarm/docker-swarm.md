---
tags: [docker, swarm, orchestrateur]
---

# Docker Swarm

Docker Swarm est un [[orchestrateur-de-conteneurs|orchestrateur de conteneurs]] intégré à Docker. Swarm fonctionne autour de différents types de [[noeuds-swarm|nœuds]] : les [[manager-swarm|managers]], et les [[workers-swarm|workers]].

![[Pasted image 20240314171317.png]]

Les Managers fournissent des tâches aux workers à travers les notions de [[services-docker-swarm|services]] et de [[tasks-docker-swarm|tasks]].

![[Pasted image 20240318165358.png]]

## Historique et activation

Dans les versions de Docker avant la 1.12 (été 2016), il s'agissait d'un add-on à Docker, s'exécutant au sein d'un conteneur Docker, automatisant les commandes Docker pour gérer le cycle de vie des conteneurs.

**A partir de la version 1.12**, Swarm est devenu un toolkit (un ensemble de librairies) **directement intégré au daemon** Docker : **SwarmKit**.

Pour éviter des **conflits** et effets de bord avec d'**autres orchestrateurs** fonctionnant avec Docker, **Swarm est désactivé par défaut**. Il faut d'abord l'[[initialisation-de-docker-swarm|activer]] avant d'exécuter des commandes Swarm, à l'aide de la commande ``docker swarm init``.

## Voir aussi

- [[orchestrateur-de-conteneurs]]
- [[initialisation-de-docker-swarm]]
- [[noeuds-swarm]]
- [[manager-swarm]]
- [[workers-swarm]]
- [[services-docker-swarm]]
- [[tasks-docker-swarm]]
