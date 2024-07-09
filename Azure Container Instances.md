
Azure Container Instances est un type de [[Resource Azure|ressource]] Azure permettant de **déployer** et **exécuter** des conteneurs Docker, sans avoir à s'occuper de l'**infrastructure sous-jacente**.

Lors de la création d'une Container Instance, une [[Docker registry|source d'image]] doit être spécifiée : 
- Quickstart images : Azure propose des images de base 
	- Un conteneur "Hello World" Linux
	- Un serveur Web Linux (Alpine)
	- Un serveur Web Windows
- [[Azure Container Registry]] (il faut pour cela avoir **activé le user admin** de cette ressource)
- Other registry (par défaut, [[DockerHub]])

Une Container Instance peut implémenter un type de **réseau public** (il se voit affecter dans ce cas une **IP publique**, et possiblement un **nom DNS**), **réseau privé** (on spécifie **un VNP**, ainsi qu'un **sous-réseau**), ou **aucun réseau**.

Une fois créée, une Container Instance peut être monitorée via le portail Azure (section "**Settings/Containers**"). On peut y voir les **logs**, les **settings**, et les **événements** liés aux containers.

Note : Une Container instance fait toujours partie d'un [[Azure Container Group|container group]].