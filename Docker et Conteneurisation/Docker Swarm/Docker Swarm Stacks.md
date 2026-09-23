
Les Stacks [[Docker Swarm]] fournissent des fonctionnalités similaires à [[Docker Compose]], mais au niveau d'un swarm.

Alors que Docker Compose permet de gérer des conteneurs Docker via un fichier yaml, les Stacks Docker Swarm permettent de **spécifier** la **configuration** complète d'un **swarm** au sein d'un [[Fichier yaml|fichier yaml]], qui est ensuite consommé pour initialiser le swarm.

La commande ``docker stack deploy -c nomDeMonFichierYaml.yml nomDuStack`` permet de **déployer** un stack.

Note : contrairement à Docker Compose, un Docker Stack **ne prend pas en compte** les **commandes de build**. Il ne sert qu'à l'exécution des [[Services Docker Swarm|services]].
Les images correspondantes doivent donc **déjà être buildées**.

Exemple de fichier stack compose : 

```yaml
version: "3.9"
services:
  redis:
    image: redis:alpine
    networks:
      - frontend
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
    deploy:
      placement:
        constraints: [node.role == manager]
  vote:
    image: bretfisher/examplevotingapp_vote
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: bretfisher/examplevotingapp_result
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
  worker:
    image: bretfisher/examplevotingapp_worker
    networks:
      - frontend
      - backend
    depends_on:
      - db
      - redis
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]
  visualizer:
    image: bretfisher/visualizer
    ports:
      - 8080:8080
    stop_grace_period: 1m30s
    networks:
      - frontend
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    deploy:
      placement:
        constraints: [node.role == manager]
        
networks:
  frontend:
  backend:
  
volumes:
  db-data:
```

Note : un Stack ne peut **concerner** qu'**un seul** Swarm.

Les commandes suivantes permettent de monitorer un stack :
- ``docker stack ls`` : **liste** les **stacks**.
- ``docker stack ps`` : affiche les [[Tasks Docker Swarm|tâches]] en exécution au sein du swarm.
- ``docker stack services`` : **liste** les [[Services Docker Swarm|services]] en exécution au sein du swarm.

Note : Pour **modifier** un Docker Stack, il convient de **modifier le fichier yaml** correspondant, puis de le **redéployer** via ``docker stack deploy -c fichier.yaml memeNomQueLeStackToUpdate``, plutôt que le modifier par des commandes Docker Swarm.
Docker va déterminer les actions à mener pour faire correspondre le stack existant aux nouveaux paramètres du fichier yaml.