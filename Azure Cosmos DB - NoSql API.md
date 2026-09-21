
Un service [[Azure Cosmos DB]] créé sur la base d'une API NoSql : 
- Stocke les données au **format json**
- Permet de requêter les données via un **langage SQL**

## Structure

Une base de donnée Cosmos DB NoSql est divisée en **containers**, qui contiennent des **items**.

![[Pasted image 20240812101108.png]]

![[Pasted image 20240812101127.png]]
![[Pasted image 20240812101204.png]]
![[Pasted image 20240812101228.png]]

Au sein d'un container, les données sont réparties en différentes **partitions logiques (logical partitions)**, selon une **clé de partition**. Ici, la clé de partition choisie est le champ "category" :
![[Pasted image 20240812101624.png]]
Les partitions sont utilisées pour répartir les données de manière à **optimiser** la **scalabilité**, la **performance**, et la gestion des requêtes. Il convient de choisir une clé de partition qui **répartit** les données de la manière la plus **homogène** possible.

Un item, au sein d'un container, est donc déterminé de **manière unique** par la paire **id / valeur de la clé de partition**.


## Requêtes Cosmos DB

La syntaxe des requêtes est la même qu'en SQL. Voir [la documentation](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/query/).

## Programmation

Pour les applications .Net, le package ``Microsoft.Azure.Cosmos`` doit être intégré à l'application : 
```powershell
dotnet add package Microsoft.Azure.Cosmos
```

```csharp
using Microsoft.Azure.Cosmos;


CosmosClient ConnectDatabase()
{
	// Connection string accessible via la section "Settings/Keys" de la ressource Azure Cosmos DB
	string connectionString="AccountEndpoint=https://appaccount400040.documents.azure.com:443/;AccountKey=XZ1cEeJNLaL75oVtQMIJtTB91OXSII8yFEK1C96rj5INID8UJh2NMaxJUP1RfRVgQhBGljxW6V8TACDbEHIswA==;";
	
	return new CosmosClient(connectionString);
}

// Instantiation du client Cosmos DB NoSql
CosmosClient cosmosClient = ConnectDatabase();

// Création d'une database
string databaseId = "appdb";
Database database = await client.CreateDatabaseAsync(databaseId);

Console.WriteLine("Database created");
  
// Création d'un container
string containerId = "courses";
string partitionKey = "/category";

Container container = await database.CreateContainerAsync(containerId,partitionKey);

Console.WriteLine("Container created");

// Ajout d'un item
string databaseId = "appdb";
string containerId = "courses";

Database database = cosmosClient.GetDatabase(databaseId);
Container container = database.GetContainer(containerId);

Course course = new Course(name, rating, category);
ItemResponse<Course> item = await container.CreateItemAsync<Course>(course,new PartitionKey(course.category));

Console.WriteLine(item.StatusCode);

// Récupération d'items
QueryDefinition queryDefinition = new QueryDefinition("SELECT * FROM courses");

using(FeedIterator<Course> feedIterator=container.GetItemQueryIterator<Course>(queryDefinition))
 {
	while(feedIterator.HasMoreResults)
	{
		FeedResponse<Course> response = await feedIterator.ReadNextAsync();
		foreach(var item in response)
		{
			Console.WriteLine($"Course ID : {item.id}");
			Console.WriteLine($"Course Name : {item.name}");
			Console.WriteLine($"Course Rating : {item.rating}");
			Console.WriteLine($"Course Category : {item.category}");
		}
	}
}

// Suppression d'item
await container.DeleteItemAsync<Course>("someId",new PartitionKey("Category"));
Console.WriteLine("Course deleted");

// Update d'un item
double newRating = 4.9;
await container.PatchItemAsync<Course>("someId",new PartitionKey("Category"),
new[] {PatchOperation.Replace("/Rating", newRating)});

Console.WriteLine("Rating Updated");

// Appel d'une procédure stockée
var result = await container.Scripts.ExecuteStoredProcedureAsync<string>("storedProcName",new PartitionKey("Certification"),new[] {courses});

Console.WriteLine(result);
```

**Note** : Il est possible de coder des **procédures stockées** en **javascript**, qui sont exécutables par du code (voir ci-dessus), ou par une autre manière (Portal, CLI,..)
Il est également possible de coder des **triggers**, également en javascript.

Exemple de procédure stockée : 
```javascript
function createItems(items) {
    var context = getContext();        
    var response = context.getResponse();
    
    if(!items)
    {        
		response.setBody("Error: Items are undefined");
		return;
    }
    var numOfItems=items.length;
    checkLength(numOfItems);

    for(let i=0;i<numOfItems;i++)
    {
        createItem(items[i]);
    }

    response.setBody("Items added to collection");    
    
    function checkLength(itemLength)
    {
		if(itemLength==0)    
         {
            response.setBody("Error: There are no items to add");    
            return;
         }
    }      

    function createItem(item)
    {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
        collection.createDocument(collectionLink,item);
    }
}
```

Voir [la documentation](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/how-to-write-stored-procedures-triggers-udfs?tabs=javascript) à ce sujet.

## Détection des changements

La gestion des [change feeds](https://learn.microsoft.com/en-us/azure/cosmos-db/change-feed) peut être mis en place : 
- En appelant une [[Azure Functions|Azure Function]] lors du changement
- Ou en appelant un [change feed processor](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/change-feed-processor?tabs=dotnet).

