
Un service [[Azure Cosmos DB]] créé sur la base d'une API Azure Table Storage stocke les données sous forme de **tableaux** : 
- Tous les types de **colonnes** sont des **types primitifs** (contrairement au NoSql, les données ne sont pas arborescentes)
- Les colonnes **PartitionKey** et **RowKey** sont obligatoires, et jouent le rôle de PartitionKey et id dans les données [[Azure Cosmos DB - NoSql API|NoSql]].
Rappel des terminologies :

## Programmation

Au sein d'une application .Net, il est nécessaire d'ajouter le package ``Azure.Data.Tables`` : 
```dotnet
dotnet add package Azure.Data.Tables
```

```csharp
using Azure.Data.Tables;
using tableapi;

await AddEntity(new Course{Name="AZ-104 Azure Administrator",PartitionKey="Certification",RowKey="C01",Rating=4.7});

await AddEntity(new Course{Name="Learning Kubernetes",PartitionKey="Software",RowKey="C02",Rating=4.6});

await AddEntity(new Course{Name="AZ-204 Azure Developer",PartitionKey="Certification",RowKey="C03",Rating=4.8});

TableClient GetTable()
{
    string connectionString="DefaultEndpointsProtocol=https;AccountName=tableaccount44434;AccountKey=RcEsk9QRqiBfSFbjgiFKmozLN7Dmy84ol3MjcQdK2iv3EzjFYpnoPgeS9SbUyu8jzne081HEiExlACDbypXemw==;TableEndpoint=https://tableaccount44434.table.cosmos.azure.com:443/;";

    TableServiceClient tableServiceClient = new TableServiceClient(connectionString);

    string tableName="courses";
    return tableServiceClient.GetTableClient(tableName);
}

// Ajout d'une entité à la table
async Task AddEntity(Course course)
{
    TableClient tableClient=GetTable();
    await tableClient.AddEntityAsync<Course>(course);

    Console.WriteLine("Entity Added");
}
```