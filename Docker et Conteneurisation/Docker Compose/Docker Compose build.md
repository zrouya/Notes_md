
Dans un fichier docker-compose, au sein d'un service, il est possible de spécifier des paramètres de build d'image.

Exemple : 
```yaml
services:
  wordpress:
    image: wordpress
    build:
      context: . # <- build context
      dockerfile: wordpress.dockerfile # <- Docker file name for the build
```

Lors de l'exécution de ``docker compose up``, Docker compose va **vérifier le cache** des images locales, et, **s'il ne trouve pas l'image** correspondant au service, va **builder l'image** avec les paramètres spécifiés.
Il est cependant possible de **forcer le build** avec les commandes ``docker compose build`` ou ``docker compose up --build``. 

Voir https://docs.docker.com/compose/compose-file/build/