
Un Azure Container Group est un regroupement de conteneurs Azure, qui partagent **le même hôte**.
Lorsqu'une ressource [[Azure Container Instances]] crée une instance de conteneur, elle le crée toujours au sein d'un **Container Group**.
Par exemple, via [[Azure CLI]], pour créer un **conteneur unique**, utiliser la commande (avec exemples de paramètres) : 
```bash
az container create --resource-group myResourceGroup --name mycontainer --image myimage:latest --cpu 1 --memory 1.5 \ --ports 80``.
```

Pour créer un Container group avec **plusieurs conteneurs**, il faut créer un [[Fichier yaml|fichier yaml]], à passer en paramètre de la commande ``az container create --resource-group myResourceGroup --file monFichier.yaml``.
Ce fichier doit avoir la structure suivante : 

```yaml
apiVersion: '2018-10-01'
location: eastus
name: monContainerGroup
properties:
  osType: Linux
  containers:
    - name: conteneur1
      properties:
        image: monimage1:latest
        resources:
          requests:
            cpu: 0.5
            memoryInGb: 1.0
    - name: conteneur2
      properties:
        image: monimage2:latest
        resources:
          requests:
            cpu: 0.5
            memoryInGb: 1.0
    ipAddress:
	    type: Public
	    ports: 80
	imageRegistryCredentials:
		- server: myAzureRegistry.azurecr.io
		  username: companyregistry
		  password: password
type: Microsoft.ContainerInstance/containerGroups
restartPolicy: OnFailure
```
