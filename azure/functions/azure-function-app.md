---
tags: [azure, functions, paas]
---

# Azure Function App

Azure Function App est un type de [[resource-azure|ressource]] Azure de type [[platform-as-a-service-paas|PaaS]] qui permet de déployer des fonctionnalités sous forme de **fonctions**, sans se soucier de l'**infrastructure** sous-jacente, gérée par Azure. Il est notamment pratique de mettre en place des Azure Functions dans le cas où une fonction est susceptible d'**être appelée par plusieurs applications**.

## Runtimes disponibles

De nombreux **runtimes et langages** sont disponibles :
- **.Net**
- **NodeJs**
- **Python**
- **Java**
- **PowerShell Core**
- **Custom Handler** : solution pour héberger du code d'une **stack non nativement gérée** par Azure Function App. Voir [configuration d'un Custom Handler](https://learn.microsoft.com/en-us/azure/azure-functions/functions-custom-handlers).

## Création de la ressource

Lors de la création de la ressource correspondante, plusieurs éléments doivent être spécifiés :
- Un [[azure-storage-accounts|storage account]] pour la **persistance des données**
- Le **système d'exploitation** hébergeant la ou les fonctions
- Un **type de plan** :
	- **Consumption (serverless)** : on ne paie qu'à l'utilisation de la fonction
	- **Functions premium**
	- **[[azure-app-service-plan|Plan App Service]]**

Une fois la ressource créée, on peut y ajouter des [[azure-functions|fonctions]] via la section **Functions/Functions** du portail Azure.

## Voir aussi

- [[azure-functions]]
- [[azure-storage-accounts]]
- [[platform-as-a-service-paas]]
