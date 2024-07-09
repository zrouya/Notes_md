
Il est possible de consulter l'ensemble de l'espace disque occupé par les images, conteneurs, volumes, et build cache [[Docker]] en exécutant la commande : 
``docker system df``

Pour libérer de l'espace disque, utiliser la commande 'prune' :
- ``docker system prune`` pour l'ensemble des éléments non utilisés
- ``docker image prune``
- ``docker container prune``
- ``docker volume prune``

