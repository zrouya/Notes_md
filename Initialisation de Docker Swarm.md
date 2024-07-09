
L'initialisation d'un Swarm Docker s'effectue via la commande ``docker swarm init``. 

Cette commande : 
- Crée un premier [[Manager Swarm|noeud manager]]
- Initialise la base de données Raft (base cryptée)
- Initialise le fournisseur de certificats
- Affecte un certificat au noeud manager principal
- Génère les jetons de rattachement de nouveaux noeuds