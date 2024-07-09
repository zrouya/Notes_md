

La création d'une [[Azure Functions|Azure Function]] de type HTTP trigger génère une fonction selon un **template** prédéfini.

Par exemple, pour une fonction Azure écrite en C#, Azure générera un fichier **run.csx** ([[CSharp Script]]), qu'il est possible d'**éditer**, de **tester**, et d'**exécuter** directement via le portail (à partir de la section "**Developer/Code + Test**", puis l'onglet "**Test/Run**") :

```csharp
// Import de la librairie externe Newtonsoft.Json
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
	//...
	//Récupération du Query String Parameter "name"
	string name = req.Query["name"]; 
	//Récupération du body de la requête
	string requestBody = await new StreamReader(req.Body).ReadToEndAsync(); 
	dynamic data = JsonConvert.DeserializeObject(requestBody);
	name = name ?? data?.name;
}
```

Note : 
Azure fournit une **Url de requête** de la fonction, avec un **query string parameter** "**code**", désignant le code de la fonction. Il suffit de chaîner les query string parameters à la suite.
