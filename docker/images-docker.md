---
tags: [docker, images]
---

# Images Docker

Une image Docker est constituée de l'ensemble des **fichiers binaires** de l'application conteneurisée, de l'ensemble de ses **dépendances**, et des **metadata** de l'application (instructions pour la lancer), consultables via ``docker image inspect [Image]``. Contrairement à une VM, une image Docker **ne contient pas de kernel ou d'OS** : ceux-ci sont **fournis par l'hôte**.

Chaque image se voit attribuer un **SHA hash unique**, qui l'identifie de manière unique, y compris lorsqu'elle est hébergée dans n'importe quel [[docker-registry|registry Docker]].

Pour obtenir la liste des images en local sur un host, exécuter ``docker image ls`` :

![[Pasted image 20240211110952.png]]

Une image Docker est décrite par un [[docker-files|fichier Docker]] qui liste les commandes à effectuer pour builder un conteneur. Chaque commande de ce fichier constitue un [[layers-images-docker|layer]], lesquels « s'empilent » les uns sur les autres. Une image Docker est [[build-des-images-docker|construite]] à partir du fichier Docker en exécutant la commande ``docker image build``.

Les images Docker ont donné lieu au [[standard-oci|standard OCI]].

## Voir aussi

- [[docker-files]]
- [[build-des-images-docker]]
- [[layers-images-docker]]
- [[docker-registry]]
- [[standard-oci]]
