
Pour builder une image Docker, utiliser la commande :
``docker image build [-t imageName[:tag]] context``

Cette commande :
- Génère une [[Images Docker|image]] Docker à partir du fichier DockerFile à l'emplacement spécifié par l'agument ``context`` (ex: '.' pour le répertoire courant et son contenu)
- Il est possible de **spécifier un autre Dockerfile** avec l'option ``-f``
- Comme sur l'exemple, il est possible, avec l'option ``--tag (-t)`` de spécifier un nom de repository pour l'image, et éventuellement le nom d'un tag.

Chaque commande ou stanza du Dockerfile donne lieu à la création d'un [[Layers d'images docker|layer d'image]], qui est mis en cache pour être **réutilisé entre différentes images** lors des **builds ultérieurs**.
