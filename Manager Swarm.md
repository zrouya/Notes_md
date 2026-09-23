
Les managers Swarm sont un type de [[Noeuds Swarm|noeuds]] qui assurent la **gestion du  cluster** (par exemple l'**orchestration des conteneurs**, via la notion de [[Services Docker Swarm|service]]).

Les managers déterminent les [[Tasks Docker Swarm|tâches]] à exécuter pour que le cluster (notamment ses [[Services Docker Swarm|services]]) atteigne son état désiré, et affectent ces tâches aux [[Workers Swarm|workers]] du swarm.

Les noeuds Managers utilisent l'algorithme de recherche de **consensus** [[Algorithme de consensus Raft|Raft]] pour :
- assurer la **cohérence** de l'état du swarm (par exemple en harmonisant les **logs de commandes**)
- assurer la **disponibilité** du système (par exemple en procédant à l'**élection d'un nouveau leader** en cas de défaillance du précédant).

Ils constituent le **Raft Consensus group**.

