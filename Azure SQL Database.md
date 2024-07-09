
Azure SQL Database est une [[Resource Azure|ressource]] Azure permettant de configurer un ou des serveurs de bases de données **SQL Server**, utilisables par des applications. Il s'agit d'une ressource [[Platform as a Service (PaaS)|PaaS]].

Lors de la création d'une Azure SQL Database, comme tout ressource, il est nécessaire de spécifier :
- Une**Subscription Azure**
- Un **Ressource Group**.

Pour le reste des options disponibles : 
- Un **nom de serveur** (suffixé par "**.database.windows.net**") -> [[Domain Name System (DNS)]] du serveur
- Un **mode d'authentification** ([[Azure Entra (anciennement Azure Active Directory)|Azure Entra]], anciennement **Azure Active Directory**, ou **authentification SQL**, ou les 2).
- Possibilité d'intégrer le serveur dans un [[SQL elastic pool Azure|elastic pool SQL]]
- Un **niveau de service et niveau de calcul**, basé sur les [[DTU (Database Transaction Unit) SQL Azure|DTU]] ou les [[vCore Azure|vCore]]
- Des options de **configuration réseau**
- Des options de **sécurité**

Note : Il est possible de créer une **connection string** permettant la connexion au serveur SQL dans la **configuration d'une Azure Web App** : 
![[Pasted image 20240311164950.png]]

![[Pasted image 20240311165027.png]]

Une fois cet élément de configuration créé, le connection string est accessible au sein du code de l'application via le service *IConfiguration*.