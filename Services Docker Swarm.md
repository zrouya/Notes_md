
Un service Docker Swarm est une **abstraction** de niveau supérieur qui définit le **comportement attendu des conteneurs** en termes d'image à utiliser, de commandes à exécuter, et de réplicas nécessaires.

- **Création de services** : Déploiement d'un nouveau service dans le Swarm, spécifiant l'image Docker à utiliser, le nombre de réplicas, les ports à publier, etc.
    - Exemple : `docker service create --name mon-service -p 80:80 mon-image`
- **Mise à jour de services** : Mise à jour des configurations d'un service existant, comme le changement d'image, la mise à l'échelle du nombre de réplicas, ou l'ajout de variables d'environnement.
    - Exemple : `docker service update --image mon-image:v2 --replicas 5 mon-service`

Un service Docker Swarm va donner lieu à un ensemble de [[Tasks Docker Swarm|tâches]] à effectuer, dans le but d'atteindre l'état souhaité du service.

Note : Lors de la **mise à jour d'un service** (par exemple en modifiant la liste des [[Docker Swarm Secrets|secrets]] auxquels il a accès, le service doit être **redéployé**, ce qui signifie que les **conteneurs en cours d'exécution** sont **stoppés** et **supprimés**, et que d'autres conteneurs sont créés et lancés avec la nouvelle configuration.
Docker Swarm effectue cette tâche en **essayant d'assurer** la **disponibilité** des services (notamment avec le paramètre ``--update-parallelism``).