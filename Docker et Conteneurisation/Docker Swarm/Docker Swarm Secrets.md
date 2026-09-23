
Docker, depuis la **version 1.13.1** de l'**API Docker Engine**, permet de créer et gérer des secrets, utilisables ensuite par les [[Services Docker Swarm|services]] Docker Swarm.

Les secrets Docker ne sont **pas utilisables** par des **containers** Docker en **standalone**, mais **uniquement par des services**.

## Création des secrets

Pour créer et gérer des secrets Docker, utiliser les commandes suivantes : 
- ``docker secret create my_secret ./secret.json`` pour créer un secret à partir d'un fichier
- ``printf "my super secret password" | docker secret create my_secret -`` : crée un secret directement via le CLI (le '-' permet de récupérer le standard input, donc la valeur du printf)
- ``docker secret ls`` liste les secrets
- ``docker secret inspect my_secret`` pour obtenir le détail d'un secret (sauf son contenu..).
- ``docker secret rm my_secret``

**Note** : La **création de secret** par cette manière n'est **possible** qu'au sein d'un [[Manager Swarm]].
(Cette commande ne fonctionnera pas avant une commande ``docker swarm init`` ou ``docker swarm join``).

Une fois créé, le secret est **stocké** de **manière sécurisée** dans la **base de données interne** de Docker Swarm (**Raft**). Le magasin Raft est **chiffré**, ce qui assure que les secrets sont gardés de manière sécurisée sur le disque. Les secrets sont **uniquement accessibles** aux **[[Manager Swarm|managers]]** du Swarm par défaut.

## Affectation des secrets aux services

Une fois le secret créé, il est possible d'accorder son accès à un service Swarm, soit à la création de celui-ci (``docker service create --secret my_secret my_service``), soit en mettant à jour le service (``docker service update --secret-add my_secret my_service``).
Pour supprimer l'accès à un secret : ``docker service upddate --secret-rm my_secret my_service``.

Note : Cette action entraîne un **redéploiement** du service (**arrêt**, **suppression** des **conteneurs** exécutant les tâches, puis **création** de **nouveaux** avec la nouvelle configuration)

## Accès aux secrets par les conteneurs

Docker **monte** les secrets dans les conteneurs du service sous forme de **fichiers** dans un système de fichiers **temporaire en mémoire** (`tmpfs`).
Ce système de fichiers n'est **pas persistant** et existe uniquement tant que le conteneur est en cours d'exécution, ce qui ajoute une couche de sécurité en évitant le stockage permanent des secrets sur le disque.

**Accès par les Applications** : Les secrets sont **montés** dans un **répertoire spécifique** à l'intérieur du conteneur, généralement `/run/secrets`. Les applications s'exécutant dans le conteneur peuvent lire les secrets comme des fichiers normaux depuis ce répertoire. Par exemple, pour un secret nommé `my_secret`, une application peut le lire en ouvrant le fichier `/run/secrets/my_secret`.

## Configuration des secrets dans les stacks Docker Swarm

Pour configurer les secrets dans les [[Docker Swarm Stacks|stacks]] Docker Swarm, il suffit de les déclarer à la racine du fichier yaml, puis de spécifier pour chaque service les secrets autorisés : 

```yaml
version: "3.9" # Note : the compose file version has to be greater than 3.1 

services:
  psql:
    image: postgres
    secrets:
      - psql_user
      - psql_password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/psql_password
      POSTGRES_USER_FILE: /run/secrets/psql_user

secrets:
  psql_user:
    file: ./psql_user.txt
  psql_password:
    file: ./psql_password.txt
# Use the following for external secrets (firstly created by docker secret create command)
  external_secret:
	external: true
```

Lors de la commande ``docker stack deploy -c composeFile.yml my_stack``, Docker Swarm crée les secrets, puis les services :

![[Pasted image 20240409174852.png]]

## Utilisation avec Docker Compose

A partir de la version 11 de [[Docker Compose]], il est également possible d'utiliser les **secrets** au **sein d'un compose file**, même à l'**extérieur** d'un **Swarm**.

Exemple : faire un ``docker-compose up`` avec le fichier docker compose suivant créera bien des secrets en local : 

```yaml
version: "3.9"

services:
  psql:
    image: postgres
    name: postgreserver
    secrets:
      - psql_user
      - psql_password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/psql_password
      POSTGRES_USER_FILE: /run/secrets/psql_user

secrets:
  psql_user:
    file: ./psql_user.txt
  psql_password:
    file: ./psql_password.txt
```

Bien que dans ce cas on ne **dispose pas** d'une **base de données Raft**, Docker effectuera **tout de même** le **montage du répertoire** des secrets :

``` bash
docker compose up
docker container ls
docker container exec postgreserver cat /run/secrets/psql_user
# La commande ci-dessus renverra bien le contenu du secret psql_user
```
