
Un bind mound Docker fonctionne comme un [[Volumes Docker|volume]] Docker, mais il permet de connecter un conteneur à un emplacement du système de fichier de l'hôte **qui n'est pas managé par Docker**.

Ceci permet à un conteneur d'avoir accès, pour persister les données, à un emplacement de l'hôte **accessible pas des processus tiers**.

Dans la mesure où un bind mount n'est pas managé par Docker (en terme de file system), la création d'un bind mount **ne peut pas faire partie du DockerFile**, mais **doit être spécifié au démarrage** du conteneur : 

```docker 
docker container run -d -p 80:80 --name nginx \
--mount target=bind, source=/cmp, target=/var/app
```

Note: Il est également possible de spécifier un bind mount avec l'option ``--volume ou -v``, mais cela est **déconseillé** pour des raisons de **lisibilité**, même si l'option ``--mount`` est plus verbeuse.

Note : Il est possible d'utiliser le raccourci Linux ``$(pwd)`` (**print working directory**) : 
``docker container run -d --mount type=bind, source="$(pwd)"/source/dir, target=/app``
