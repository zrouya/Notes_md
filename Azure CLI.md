
Azure CLI est un **outil** en **lignes de commande** **multiplateforme** permettant de se connecter à [[Azure]] et d’exécuter des **commandes d’administration** sur les ressources Azure.

Il s'agit du pendant [[Bash]] du module [[Azure PowerShell]], il permet d'implémenter les mêmes actions d'administration du Cloud Azure (à quelques commandes près).

Exemple de commandes Azure CLI : 

```bash
# Create an App service plan
az appservice plan create --name companyplan --resource-group app-grp --is-linux

# Create the web app
az webapp create --resource-group app-grp --plan companyplan --name dockerapp55000 --deployment-container-image-name appregistry1000122.azurecr.io/sqlapp:latest

# Turn on container logging
az webapp log config --name dockerapp55000 --resource-group app-grp --docker-container-logging filesystem

# Enable the log stream
az webapp log tail --name dockerapp55000 --resource-group app-grp
```
