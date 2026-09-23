---
tags: [azure, conteneurs, docker]
---

# Publier une image Docker vers un Azure Container Registry

Pour pusher des [[Images Docker|images]] Docker dans un [[azure-container-registry|Azure Container Registry]], il est nécessaire :
- D'**installer le [[azure-cli|CLI]]** d'Azure (exemple pour Linux, voir la [documentation d'installation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt))
- Se **logger** au Azure Container Registry, en exécutant la commande
	``az ecr login --name azureContainerRegistryName --username usr --password pwd``
- **Tagger l'image** avec le nom du serveur Azure Container Registry :
	``docker image tag myImage azureContainerRegistryName.azurecr.io/myImageName``
- **Pusher** l'image : ``docker image push azureContainerRegistryName.azurecr.io/myImageName``

Une fois ceci fait, l'image pushée est visible dans la section "**Services/Repositories**" de la ressource Azure Container Registry, dans le portail Azure.

## Voir aussi

- [[azure-container-registry]]
- [[azure-cli]]
