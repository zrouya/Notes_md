---
tags: [docker, containers, cli]
---

# Management des conteneurs Docker

Docker fournit un ensemble de commandes permettant de **monitorer** et **d'interagir** avec des conteneurs en exécution, au-delà de leur simple démarrage ou arrêt.

## Monitorer des conteneurs

- ``docker container top [Container]`` permet d'**obtenir la liste des processus** en exécution dans le conteneur spécifié.
- ``docker container inspect [Container]`` affiche la **configuration d'exécution** du conteneur, sous la forme d'un **fichier json**.
- ``docker container stats [Container...]`` permet de **monitorer les ressources utilisées** par un, des, ou tous les containers.
- ``docker container logs [Container]`` permet d'accéder aux logs d'un conteneur.

## Interagir avec des conteneurs

Pour obtenir un [[Types de shell|shell]] au sein d'un conteneur, il est possible :

- d'exécuter le conteneur en mode interactif : ``docker container run -it [Image] [Command]``
- de démarrer un conteneur existant en mode interactif : ``docker container start -ai [Container] [Command]``
- d'exécuter une commande sur un container déjà en exécution : ``docker container exec [Container] [Command]``, ex : ``docker container exec myContainer bash``

**Notes :**
- Dans les 2 premiers cas, la commande passée en paramètre est la **commande principale** du conteneur, ce qui signifie que **le conteneur sera stoppé quand cette commande sera finie** (comme c'est le cas avec un ``docker run`` classique, sauf que dans ce cas-là, il s'agit de la commande spécifiée par l'[[images-docker|image]]).
- Dans le cas de la commande ``docker container exec``, on exécute une commande additionnelle sur le conteneur, qui ne sera pas stoppé à la fin de celle-ci (car sa commande principale est celle définie par son image, ou au ``docker run``).
- L'option ``-it`` est en fait une double option : ``-i --interactive``, qui branche le **flux stdin** du conteneur, et l'option ``-t --tty`` qui alloue un pseudo tty, comme [[Bash|bash]].

## Voir aussi

- [[execution-des-conteneurs-docker]]
- [[commandes-docker]]
- [[containers-docker]]
