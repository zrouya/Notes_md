---
tags: [docker, swarm]
---

# Manager Swarm

Les managers Swarm sont un type de [[noeuds-swarm|nœuds]] qui assurent la **gestion du cluster** (par exemple l'**orchestration des conteneurs**, via la notion de [[services-docker-swarm|service]]). Les managers déterminent les [[tasks-docker-swarm|tâches]] à exécuter pour que le cluster (notamment ses [[services-docker-swarm|services]]) atteigne son état désiré, et affectent ces tâches aux [[workers-swarm|workers]] du swarm.

Les nœuds managers utilisent l'algorithme de recherche de **consensus** [[algorithme-de-consensus-raft|Raft]] pour :
- assurer la **cohérence** de l'état du swarm (par exemple en harmonisant les **logs de commandes**)
- assurer la **disponibilité** du système (par exemple en procédant à l'**élection d'un nouveau leader** en cas de défaillance du précédent)

Ils constituent le **Raft Consensus group**.

## Voir aussi

- [[noeuds-swarm]]
- [[workers-swarm]]
- [[services-docker-swarm]]
- [[tasks-docker-swarm]]
