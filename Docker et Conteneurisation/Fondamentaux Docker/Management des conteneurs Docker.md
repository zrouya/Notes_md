
## Monitorer des conteneurs

La commande ``docker container top [Container]`` permet d'**obtenir la liste des processus** en exécution dans le conteneur spécifié.

La commande ``docker container inspect [Container]`` affiche la **configuration d'exécution** du conteneur, sous la forme d'un **fichier json**.

Pour **monitorer les ressources utilisées** par un, des, ou tous les containers, utiliser la commande ``docker container stats [Container...]``.

Pour accéder aux logs d'un conteneur, exécuter ``docker container logs [Container]``.

## Interagir avec des conteneurs

Pour obtenir un [[Types de shell|shell]] au sein d'un conteneur, il est possible : 

- d'exécuter le conteneur en mode intéractif : ``docker container run -it [Image] [Command]``
- pour démarrer un conteneur existant en mode interactif : ``docker container start -ai [Container] [Command]``
- pour exécuter une commande sur un container déjà en exécution : ``docker container exec [Container] [Command]`` ex : ``docker container exec myContainer bash``

**Notes :** 
- Dans les 2 premiers cas, la commande passée en paramètre est la **commande principale** du conteneur, ce qui signifie que **le conteneur sera stoppé quand cette commande sera finie** (comme c'est le cas avec un ``docker run`` classique, sauf que dans ce cas là, il s'agit de la commande spécifiée par l'[[Images Docker|image]]).
- Dans le cas de la commande ``docker container exec``, on exécute une commande additionnelle sur le conteneur, qui ne sera pas stoppé à la fin de celle-ci (car sa commande principale est celle définie par son image, ou au ``docker run``).
- L'option ``-it`` est en fait une double option : ``-i --interactive``, qui branche le **flux stdin** du conteneur, et l'option ``-t --tty`` qui aloue un pseudo tty, comme [[Bash|bash]].

