
La mise à jour de [[Services Docker Swarm|services]] Docker se fait via la commande ``docker service update``.

[[Docker Swarm]] va effectuer la mise à jour des [[Tasks Docker Swarm|tâches]] (conteneurs) de ce service **de manière progressive** (pas toutes les tâches à la fois), pour **limiter l'indisponibilité** du service.

Note : Docker ne mettra pas à jour les conteneurs directement, mais **arrêtera**, **supprimera**,  **recréera**, et **relancera** les conteneurs correspondant.

L'exécution de la commande ``docker stack deploy -c modifiedFile.yml existingStack`` d'un stack **déjà déployé** entraînera la **mise à jour** des **services** (et autres objets Docker comme les **networks**,...) pour que les **modifications** du **fichier Docker Compose** soient prises en compte.

## Commandes service update de base

- Pour **mettre à jour** l'**image** d'un service :
	``docker service update --image newImage:3 <nom_du_service>``
- **Variables d'environnement** :
	``docker service update --env-add ENV_VAR=value --env-rm ENV_TO_RM``
- **Ports** :
	``docker service update --publish-rm 8080 --publish-add 8088:80``
	Note : il n'est **pas possible** de **mettre à jour** un port **mappé**, il faut le **supprimer** puis le **remapper**
- Modifier le **nombre de réplicas** d'un ou plusieurs services se fait avec la commande **scale** : 
	``docker service scale myService=5 otherService=8``

## Rééquilibrer un Swarm (re balancing)

Il peut arriver dans un Swarm (ayant eu beaucoup de [[Noeuds Swarm|noeuds]] rejoignant ou quittant le Swarm) que les **tâches** ne soient **pas idéalement réparties** sur tous les noeuds.
En effet, Docker Swarm ne veille pas particulièrement à ce que la charge de travail soit toujours bien répartie au sein des noeuds.

Il est cependant possible de **forcer** un **rééquilibrage** (rebalancing) d'un **service** en forçant sa **mise à jour** (même **sans modification** réelle du service), via la commande : 
``docker service update --force myService``.

Docker Swarm recréera les tâches du service, en privilégiant les noeuds les moins occupés.
