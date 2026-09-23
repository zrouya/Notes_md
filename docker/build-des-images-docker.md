---
tags: [docker, images, build]
---

# Build des images Docker

Pour builder une image Docker, on utilise la commande ``docker image build [-t imageName[:tag]] context``. Cette commande génère une [[images-docker|image]] Docker à partir du Dockerfile situé à l'emplacement spécifié par l'argument ``context`` (ex : ``.`` pour le répertoire courant et son contenu).

## Options principales

- Il est possible de **spécifier un autre Dockerfile** avec l'option ``-f``.
- L'option ``--tag (-t)`` permet de spécifier un nom de repository pour l'image, et éventuellement le nom d'un tag.

Chaque commande ou stanza du Dockerfile donne lieu à la création d'un [[layers-images-docker|layer d'image]], qui est mis en cache pour être **réutilisé entre différentes images** lors des **builds ultérieurs**.

## Voir aussi

- [[docker-files]]
- [[layers-images-docker]]
- [[build-multi-stage-images-docker]]
- [[images-docker]]
