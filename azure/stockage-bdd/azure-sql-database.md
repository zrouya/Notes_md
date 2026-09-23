---
tags: [azure, stockage, sql]
---

# Azure SQL Database

Azure SQL Database est une [[resource-azure|ressource]] Azure permettant de configurer un ou des serveurs de bases de données **SQL Server**, utilisables par des applications. Il s'agit d'une ressource [[platform-as-a-service-paas|PaaS]].

## Création

Lors de la création d'une Azure SQL Database, comme toute ressource, il est nécessaire de spécifier :
- Une **Subscription Azure**
- Un **Resource Group**

Pour le reste des options disponibles :
- Un **nom de serveur** (suffixé par "**.database.windows.net**") -> [[Domain Name System (DNS)]] du serveur
- Un **mode d'authentification** ([[azure-entra|Azure Entra]], anciennement **Azure Active Directory**, ou **authentification SQL**, ou les 2)
- Possibilité d'intégrer le serveur dans un [[sql-elastic-pool-azure|elastic pool SQL]]
- Un **niveau de service et niveau de calcul**, basé sur les [[dtu-sql-azure|DTU]] ou les [[vcore-azure|vCore]]
- Des options de **configuration réseau**
- Des options de **sécurité**

Note : Il est possible de créer une **connection string** permettant la connexion au serveur SQL dans la **configuration d'une Azure Web App** :
![[Pasted image 20240311164950.png]]

![[Pasted image 20240311165027.png]]

Une fois cet élément de configuration créé, le connection string est accessible au sein du code de l'application via le service *IConfiguration*.

## Voir aussi

- [[dtu-sql-azure]]
- [[vcore-azure]]
- [[sql-elastic-pool-azure]]
