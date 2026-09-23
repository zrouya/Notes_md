
Azure Cosmos DB est une [[Resource Azure|ressource]] Azure proposant un **service distribué** de bases de données de différents types.

Lors de la création d'un Azure Cosmos DB account, le **type d'API** implémenté doit être défini : 
- [[Azure Cosmos DB - NoSql API|NoSql]]
- MongoDB
- Cassandra
- PostgreSql
- Gremlin
- [[Azure Cosmos DB - Table API|Azure Table Storage]]

Voir [la documentation](https://learn.microsoft.com/en-us/azure/cosmos-db/).

## Azure PowerShell

Pour manager un Azure Cosmos DB account via [[Azure PowerShell]], voir l'exemple suivant : 
```powershell
Connect-AzAccount

$ResourceGroupName="app-grp"
$Location="North Europe"
$AccountName="appaccount5775674"

$Account=New-AzCosmosDBAccount -Name $AccountName -ResourceGroupName $ResourceGroupName `
-Location $Location -ApiKind Sql

$DatabaseName="appdb"
$Database=New-AzCosmosDBSqlDatabase -ParentObject $Account -Name $DatabaseName

$ContainerName="courses"
$PartitionKey="/category"

New-AzCosmosDBSqlContainer -ParentObject $Database -Name $ContainerName `
-PartitionKeyKind Hash -PartitionKeyPath $PartitionKey
```

## Consistance des données

Pour améliorer les **performances** des applicatifs, il est possible de **répliquer** des solutions de stockage dans différentes **zones**, ce qui pose un problème de [consistance des données](https://learn.microsoft.com/en-us/azure/cosmos-db/consistency-levels).

Il existe plusieurs stratégies d'arbitrage entre performance et consistance des données : 
- **Strong** : Les données retournées sont toujours les plus récentes
- **Bounded staleness** : On minimise le décalage entre les régions (une donnée ne peut être en retard de plus de K versions ou de T secondes, en fonction de l'occurrence qui arrive en premier).
- **Session**
- **Consistent prefix**
- **Eventual**
