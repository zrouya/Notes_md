
Azure Container App est une [[Resource Azure|ressource Azure]] serverless, permettant d'exécuter des applications [[Containers Docker|conteneurisées]] sans se soucier de l'infrastructure ou de l'orchestration.

Elle repose sur [[Kubernetes]].

Pour créer des container apps, utiliser le genre de commande [[Azure CLI]] ci-dessous :
```powershell
az containerapp env create --name "learningapp-env" --resource-group "app-grp" --location "North Europe"

az containerapp create --name "mysql-service" --resource-group "app-grp" --environment "learningapp-env"--image appregistry45545.azurecr.io/mysql-image:latest --target-port 3306 --exposed-port 3306 --transport tcp --ingress 'internal' --registry-server appregistry45545.azurecr.io --query properties.configuration.ingress.fqdn

az containerapp create --name "learningapp-service" --resource-group "app-grp" --environment "learningapp-env" --image "appregistry45545.azurecr.io/learningapp:latest" --target-port 8080 --ingress 'external' --registry-server appregistry45545.azurecr.io --query properties.configuration.ingress.fqdn
```

Azure créera différentes ressources pour exécuter et orchestrer les applications correspondantes.