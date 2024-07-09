
Le Dockerfile est le **patron de construction** du conteneur, il est constitué de la liste des commandes, ou **stanza** à exécuter pour lancer le conteneur.
Il constitue le squelette de l'[[Images Docker|image Docker]].

Exemple :

```docker
#This is a sample Image 
FROM ubuntu 
RUN apt-get update 
RUN apt-get install –y nginx 
CMD [“echo”,”Image created”]
```

Voir les commandes principales : https://docs.docker.com/engine/reference/builder/ :

### Commande FROM

- Image sur laquelle est basée l'image du Dockerfile
- Ce stanza est **obligatoire**, tout Dockerfile doit **commencer par une commande FROM**
- Généralement, elle fait référence à une **distribution Linux minimale** (ex: Debian, ou, encore mieux, alpine)
- Pour une image **sans image de base**, commencer par ``FROM scratch``

### Commande ENV

- Modification de la valeur d'une **variable d'environnement** du conteneur
- Permet de passer des paires clé/valeurs utiles au lancement et à l'exécution du package.
- Cette méthode est préférée au passage direct de paires clé/valeurs, car elle est compatible avec tous les environnements et OS.

### Commande RUN

- Permet d'exécuter toute commande shell accessible dans le conteneur

### Commande CMD

- Spécifie la **commande principale** du conteneur, celle qui est exécutée par défaut lors d'un ``docker container run``, et qui stoppe le conteneur lorsqu'elle est terminée.
- Ce stanza est également **obligatoire** dans un Dockerfile, cependant, il **peut être hérité** de l'image spécifiée dans le stanza **FROM**. Il n'apparaît donc pas toujours explicitement dans un Dockerfile.


Chaque ligne (commande) du fichier docker donne lieu à un [[Layers d'images docker|layer de l'image]], généré lors de la [[Build des images Docker|construction de l'image Docker]].

Il est donc important, pour bénéficier au maximum du système de cache des layers, de **bien ordonner les stanza** dans le Dockerfile :
Les commandes **les moins susceptibles de changer en premier**, celles **les plus susceptibles de changer en dernier**.