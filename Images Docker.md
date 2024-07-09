
Une image Docker est constituée : 
- De l'ensemble des **fichier binaries** de l'application conteneurisée
- De l'ensemble de ses **dépendances**
- Des **metadata** de l'application et des **instructions pour lancer** l'application, que l'on peut consulter en exécutant ``docker image inspect [Image]``

Contrairement à une VM, une image Docker **ne contient pas de kernel ou d'OS**, ceux-ci sont **fournis par l'hôte**.

Chaque image se voit attribuer un **SHA hash unique**, qui identifie l'image de manière unique, y compris lorsqu'elle sont hébergées dans n'importe quel [[Docker registry|registry Docker]].

Pour obtenir la liste des images en local sur un host, exécuter ``docker image ls`` :

![[Pasted image 20240211110952.png]]

Une image Docker est décrite par un [[Docker files|fichier Docker]] qui liste les commandes à effectuer pour builder un conteneur. 

Chaque commande de ce ficher constitue un [[Layers d'images docker|layer]], lequels "s'empilent" les uns sur les autres.

Une image Docker est [[Build des images Docker|construite]] à partir du fichier Docker en éxecutant la commande `docker image build`.

Les images Docker ont donné lieu au [[Standard OCI]].