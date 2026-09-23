
Depuis la **version 1.12** de [[Docker|docker]] (milieu 2016), la fonctionnalité **Healthcheck** permet à docker de **vérifier l'état** de fonctionnement des [[Containers Docker|conteneurs]].

Il s'agit d'une **commande** que Docker **exécute (commande ``exec``)** dans un **conteneur** pour vérifier si celui-ci est dans un état **healthy (retourne 0)** ou **unhealthy (retourne 1)**.
**Avant** le **premier healthcheck**, le conteneur est considéré comme dans l'état **starting**.

Cette fonctionnalité est disponible :
- Comme option de la commande ``docker container run``
- Dans les [[Docker files|Docker files]]
- Dans les fichiers [[Fichier Docker compose|Docker compose]]
- En option lors de la **création** ou de la **mise à jour** des [[Services Docker Swarm|services]] Docker Swarm
	(commandes ``docker service create`` et ``docker service update``)
- Au sein des [[Fichier yaml|fichiers yaml]] permettant de créer des [[Docker Swarm Stacks|stacks]] Docker Swarm.

Une fois les **Healthchecks configurés** (par l'une ou l'autre des manières ci-dessus), la commande ``docker container ls`` indiquera pour chaque conteneur son statut, et la commande ``docker container inspect`` indiquera les retours des 5 derniers Healthchecks du conteneur.

``docker container run`` ne fera **aucune action** si un conteneur est unhealthy, c'est le rôle des [[Services Docker Swarm|services]] Docker Swarm de **démarrer une nouvelle tâche**.
Note : la commande ``docker service update`` **attendra** que le Healthcheck d'une tâche (du conteneur associé) soit valide **avant de mettre à jour** la tâche **suivante** (avant de **remplacer** le **conteneur** suivant).

## Utilisation avec la commande docker run

```bash
docker container run /
	# Un conteneur n'est pas obligé d'exposer des ports extérieur, le healthcheck se fait au sein du conteneur.
# L'option -f (fail) demande à curl de gérer les erreurs de manière silencieuse, plutôt que de retourner une page d'erreur (la commande de healthcheck doit juste retourner 1 ou false en cas d'erreur, voir ci-dessous)
	# le '|| false' s'assure du retour de la commande de healthcheck. Ce snipnet peut être remplacé par '|| exit 1'
	--health-cmd="curl -f localhost:9200/_cluster/health || false" /
	--health-interval=5s / # Intervalle entre 2 healthchecks (par défaut 30s)
	--health-retries=3 / # Nombre d'essais à 1 avant de considérer le conteneur comme unhealthy
	--health-timeout=2s / # Si la commande n'a pas de retour après ce timeout, le conteneur est considéré comme unhealthy
	--health-start-period=15s / # Temps au démarrage du conteneur pendant lequel il ne sera pas considéré comme unhealthy, même si les healthchecks sont à 1 pendant cette période
	elasticsearch:2 
```

## Healthchecks dans un Docker file

```dockerfile
HEALTHCHECK curl -f http://localhost/ || false
# Ou, si on veut spécifier des options, ajouter CMD avant la commande :
HEALTHCHECK --timeout=2s --interval=3s --retries=3 /
	CMD curl -f http://localhost3 || exit 1
	# Note : il ne s'agit pas ici de 2 stanza différentes, i.e. le '/' en bout de ligne
	# Rappel : le '|| exit 1' est équivalent à '|| false'
```

Exemples : 

- pour une image **Nginx** servant un **site web statique**, tester l'**URL par défaut** :
```dockerfile
FROM nginx:1.13

HEALTHCHECK --interval=10s --timeout=3s CMD curl -f http://localhost || false
```

- Pour un **serveur Postgres** :
```dockerfile
FROM postgres
HEALTHCHECK pg_isready -U postresUser || exit 1
# Spécifier un User existant avec -U
```

## Healthckecks dans un fichier Compose/Stack

```yaml
version: "2.1" # Version minimum pour les Healthchecks
services:
	web:
		image: nginx
		healthcheck:
			test: ["CMD", "curl", "-f", "http://localhost"]
			interval: 1m30s
			timeout: 10s
			retries: 3
			start_period: 1m # version 3.4 minimum
```

