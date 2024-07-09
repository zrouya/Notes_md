

Pour pusher des [[Images Docker|images]] Docker dans un [[Azure Container Registry]], il est nécessaire : 
- D'**installer le [[Azure CLI|CLI]]** d'Azure (exemple pour Linux, voir la [documentation d'installation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt))
- Se **logger** au Azure Container Registry, en exécutant la commande
	``az ecr login --name azureContainerRegistryName --username usr --password pwd``
- **Tagger l'image** avec le nom du serveur Azure Container Registry :
	``docker image tag myImage azureContainerRegistryName.azurecr.io/myImageName``
- **Pusher** l'image : ``docker image push azureContainerRegistryName.azurecr.io/myImageName``

Une fois ceci fait, l'image pushée est visible dans la section "**Services/Repositories**" de la ressource Azure Container Registry, dans le portail Azure.