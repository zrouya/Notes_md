---
tags: [moc, docker]
---

# Docker Swarm

Orchestrateur de conteneurs natif de Docker : noeuds, services, tâches et fonctionnalités associées (secrets, routing mesh, stacks).

## Notes

- [[Docker Swarm]] — orchestrateur intégré, managers et workers
- [[Initialisation de Docker Swarm]] — commande `docker swarm init`
- [[Noeuds Swarm]] — machine avec un Docker engine faisant partie du swarm
- [[Manager Swarm]] — noeuds assurant la gestion du cluster
- [[Workers Swarm]] — noeuds exécutant les tâches du cluster
- [[Services Docker Swarm]] — abstraction définissant le comportement attendu des conteneurs
- [[Tasks Docker Swarm]] — instances d'un service exécutées sur un noeud
- [[Mise à jour de Services Docker Swarm]] — mise à jour progressive des tâches d'un service
- [[Docker Swarm Routing Mesh]] — routage réseau vers les tâches d'un service
- [[Docker Swarm Secrets]] — gestion des secrets utilisables par les services Swarm
- [[Docker Swarm Stacks]] — équivalent de Docker Compose au niveau d'un swarm
- [[Algorithme de consensus Raft]] — algorithme de consensus utilisé par la base de données interne des managers
