
Azure App Configuration est une [[Resource Azure|ressource]] Azure permettant de mettre à disposition des **paires clé/valeur** à une [[Azure Web Application|application]] Azure, **de manière centralisée**.

Pour une application [[ASP.NET Core|ASP .Net Core]], il est possible, via le package Nuget Microsoft.**Azure.AppConfiguration.AspNetCore**, d'accéder à la Config Azure via sa **connection string** (voir la section "**Settings/Access settings**" de la ressource App Configuration).
Dans le code de build de l'application, utiliser la méthode ``AddAzureAppConfiguration``  pour lier la configuration de l'application à la Azure App Configuration : 

```csharp
//Program.cs (ou startup.cs) -> Définition du middleware
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();

var azureAppConfConnectionString = ""; //Récupérée dans Access Settings de la ressource Azure
builder.Configuration.AddAzureAppConfiguration(azureAppConfConnectionString);
```

Une fois la configuration liée, elle peut être accessible au sein d'une application Web via les objets injectés de type *IConfiguration* : 

```csharp
public class MyClass
{
	public MyClass(private IConfiguration _configuration) { }
	
	public void MyMethod()
	{
		var someConfValue = _configuration.GetValue("someKey");
	} 
}
```

