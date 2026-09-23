---
tags: [docker, swarm]
---

# Initialisation de Docker Swarm

L'initialisation d'un Swarm Docker s'effectue via la commande ``docker swarm init``.

## Actions effectuées

- Crée un premier [[manager-swarm|nœud manager]]
- Initialise la base de données Raft (base cryptée)
- Initialise le fournisseur de certificats
- Affecte un certificat au nœud manager principal
- Génère les jetons de rattachement de nouveaux nœuds

## Voir aussi

- [[docker-swarm]]
- [[manager-swarm]]
- [[noeuds-swarm]]
