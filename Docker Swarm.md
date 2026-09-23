
Docker Swarm est un [[Orchestrateur de conteneurs]] intégré à Docker.

Swarm fonctionne autour de différents types de [[Noeuds Swarm|noeuds]] : les [[Manager Swarm|managers]], et les [[Workers Swarm|workers]].

![[Pasted image 20240314171317.png]]


Les Managers fournissent des tâches aux workers à travers les notions de [[Services Docker Swarm|services]] et de [[Tasks Docker Swarm|tasks]]

![[Pasted image 20240318165358.png]]


Dans les versions de Docker avant la 1.12 (été 2016), il s'agissait d'un add-on à Docker, s'exécutant au sein d'un conteneur Docker, automatisant les commandes Docker pour gérer le cycle de vie des conteneurs.

**A partir de la version 1.12**, Swarm est devenu un toolkit (un ensemble de librairies) **directement intégré au dameon** Docker : **SwarmKit**.

Pour éviter des **conflits** et effets de bords avec d'**autres orchestrateurs**, fonctionnant avec Docker, **Swarm est désactivé par défaut**. Il faut d'abord l'[[Initialisation de Docker Swarm|activer]] avant d'exécuter des commandes Swarm, à l'aide de la commande ``docker swarm init``.