
Azure Container Instances est un type de [[Resource Azure|ressource]] Azure permettant de **déployer simplement** et **exécuter** des [[Containers Docker|conteneurs]] Docker, sans avoir à s'occuper de l'**infrastructure sous-jacente**.

Cette ressource est adaptée à des **besoins simples**, des tâches ponctuelles, ne nécessitant **pas d'orchestration** de conteneur, ou de de **mise à l'échelle** applicative.

Pour une gestion plus puissante des conteneurs, utiliser plutôt des [[Azure Container App|container apps]].

Lors de la création d'une Container Instance, une [[Docker registry|source d'image]] doit être spécifiée : 
- Quickstart images : Azure propose des images de base 
	- Un conteneur "Hello World" Linux
	- Un serveur Web Linux (Alpine)
	- Un serveur Web Windows
- [[Azure Container Registry]] (il faut pour cela avoir **activé le user admin** de cette ressource)
- Other registry (par défaut, [[DockerHub]])

Puis, on spécifie l'[[Images Docker|image Docker]] sur laquelle est basée le conteneur qui va être exécuté.

Note : Une Container instance fait toujours partie d'un [[Azure Container Group|container group]].

Via le [[Azure CLI]] : 
```bash
az container create --resource-group myResourceGroup --name mycontainer --image myimage:latest --cpu 1 --memory 1.5 \ --ports 80``.
```

Une Container Instance peut implémenter un type de **réseau public** (il se voit affecter dans ce cas une **IP publique**, et possiblement un **nom DNS**, ou des **Ports publiés**), **réseau privé** (on spécifie **un VPN**, ainsi qu'un **sous-réseau**), ou **aucun réseau**.

Une fois créée, une Container Instance peut être monitorée via le portail Azure (section "**Settings/Containers**"). On peut y voir les **logs**, les **settings**, et les **événements** liés aux containers.