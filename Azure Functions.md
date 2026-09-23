
Les Azure Functions sont les éléments d'une ressource [[Azure Function App]], affichés dans la section **Functions/Functions** du portail Azure, après avoir sélectionné la ressource Function App correspondante.

![[Pasted image 20240321113810.png]]

Une fonction Azure doit avoir un **type de trigger** défini : 
- **[[HTTP Trigger Azure Function|HTTP trigger]]** : la fonction est requêtable par HTTP(S)
- **Timer trigger** pour les fonctions s'exécutant automatiquement
- **Azure queue storage trigger** : la fonction s'exécute à la réception d'un message Azure Storage queue
- **Azure Service Bus queue trigger** : la fonction s'exécute à la réception d'un message Azure Service Bus queue

Note : 
- Il est possible d'effectuer le développement d'une fonction Azure sur **Visual Studio (2022)**. Pour ce faire, créer un nouveau **projet de type Azure Function**.
- Une fois créée, il est possible de publier cette fonction sur Azure, de la même manière que pour une WebApp.
- Il est également possible d'**ajouter une fonction** Azure à la [[Azure Function App]] **via Visual Studio**. Pour ce faire, il suffit de créer une nouvelle classe au projet, de type **"Azure function"**.
- Comme pour une Azure Web App, il est possible d'ajouter des **items de configuration** à une Azure Function (section "**Settings/Configuration**"), y compris des **connection strings**, dans l'onglet correspondant.
	Ces éléments de configuration seront accessible dans le code de l'Azure Function **en tant que variables d'environnement**. Par exemple : 
	```csharp
	var connString = Environment.GetEnvironmentVariable("SQLAZURECONNSTR_MonNomDeConfigItem");
	// Ici la variable d'environnement est préfixée par 'SQLAZURECONNSTR_'
```

