
Un volume Docker est un **emplacement spécial** dans le système de fichier, dont le contenu **n'est pas supprimé lors de la suppression du conteneur**.

Un volume Docker doit être [[Gestion des données Docker|supprimé manuellement]].

On définit un volume docker par une stanza ``VOLUME`` dans le DockerFile :
```docker
VOLUME /var/lib/mysql
```

Cette commande va créer un [[Bind mounts Linux|bind mount]] linux, un mappage entre le répertoire du conteneur (ici, var/lib/mysql), et un répertoire de l'hôte.
Les volumes Docker sont stockés, au sein de l'hôte, dans un emplacement **géré par Docker** (sous linux, **var/lib/docker/volumes**).
**Un process hors Docker ne devrait pas modifier cette emplacement du système de fichier**. Pour cela, il convient d'utiliser plutôt un [[Bind mounts Docker|bind mount docker]].

Après l'exécution du conteneur en question, ce nouveau volume pourra être retrouvé par la commande ``docker volume ls``.

**Note :** l'option ``--volume (-v)`` de la commande ``doker run`` permet d'obtenir le même résultat : 
``docker container run -d --name mysqlcontainer -v /var/lib/mysql mysql

### Volumes nommés

Dans la mesure où les volumes Docker persistent au delà des conteneurs (même quand ceux-ci sont supprimés), il est intéressant d'attribuer un nom à ces volume. Pour cela, il suffit de préfixer l'option volume ci-dessus :

``docker container run -d --name mysqlnamedvolume -v volumeName:/var/lib/mysql mysql``

