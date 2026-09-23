
La [[Commandes Docker|commande]] ``docker container run <image>`` (ou ``docker run``) permet de lancer un [[Containers Docker|container]] : 
- Si l'image du container spécifiée n'est pas trouvé localement, elle sera recherchée et téléchargée à partir d'un [[Docker registry|registry docker]], par défaut [[DockerHUB]].
- La commande ``docker container run`` va **systématiquement créer un nouveau container.**

Pour exécuter un container déjà existant, utiliser la commande ``docker start <container id/name>``.

Pour arrêter un container, ``docker stop``.

Pour lister les containers **en exécution**, ``docker container ls`` (ou anciennement ``docker ps``).

Pour lister la totalité des containers (même ceux arrêtés), ``docker container ls -a``.

Pour supprimer un container, ``docker container rm <container id/name>``
- Il est possible de chainer les id ou noms de containers. ex : ``docker container rm cont1 aeFG myHost``.
- Pour des raisons de sécurité, cette commande ne peut pas supprimer un conteneur en exécution. Pour ce faire, ajouter l'option --force (-f) : ``docker container rm -f myHost``.

### Gestion des données persistantes

Les containers Dockers étant, par principe, **[[Immutable Infrastructure|immutables]]** et **éphémères**, se pose le problème des **données persistantes (persistent data)**.

Les **données manipulées par un conteneur** lors de son cycle de vie (le [[UFS Layer|layer UFS]] correspondant), ne sont pas **perdues** lorsque le conteneur est stoppé, **uniquement lorsqu'il est supprimé**.

Pour gérer les **data partagées** par des conteneurs (ex : les **données uniques** d'une application), on utilise les [[Volumes Docker|volumes]], [[Bind mounts Docker|bind mounts]], et les [[Tmpfs mounts Docker|tmpfs mounts]] (mappages de répertoire) (voir https://docs.docker.com/storage/):

![[Pasted image 20240213113835.png]]

Note : les conteneurs s'exécutent à partir de leur image Docker en implémentant le concept de [[Copy and Write]].