
Azure Function App est un type de [[Resource Azure|ressource]] Azure de type [[Platform as a Service (PaaS)|PaaS]] qui permet de déployer des fonctionnalités sous forme de **fonctions**, sans se soucier de l'**infrastructure** sous-jacente, géré par Azure.

Il est notamment pratique de mettre en place des Azure functions dans le cas où une fonction est susceptible d'**être appelée par plusieurs applications**.

De nombreux **runtimes et langages** sont disponibles :
- **.Net**
- **NodeJs**
- **Python**
- **Java**
- **PowerShell Core**
- **Custom Handler** : solution pour héberger du code d'une **stack non nativement gérée** par Azure Function App. Voir [configuration d'un Custom Handler](https://learn.microsoft.com/en-us/azure/azure-functions/functions-custom-handlers). 

Lors de la création de la ressource correspondante, plusieurs éléments doivent être spécifiés : 
- Un [[Azure Storage Accounts|storage account]] pour la **persistance des données**
- Le **système d'exploitation** hébergeant la ou les fonctions
- Un **type de plan** : 
	- **Consumption (serverless)** : on ne paie qu'à l'utilisation de la fonction
	- **Flex Consumption**
	- **Functions premium** 
	- **App Service** : intégration de la function à un [[Azure App Service Plan|Plan App Service]] (en termes tarification également)
	- **Container App Environment** 

Une fois la ressource créée, on peut y ajouter des [[Azure Functions|fonctions]] via la section **Functions/Functions** du portail Azure.

