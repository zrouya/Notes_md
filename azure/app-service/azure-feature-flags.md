---
tags: [azure, app-service, feature-flags]
---

# Azure Feature Flags

Les feature flags Azure sont un cas particulier de [[azure-app-configuration|configuration]] Azure, utilisés pour **activer ou désactiver une feature** au sein d'une [[azure-web-application|application]] Azure.

## Création dans le portail

Pour ce faire, il suffit d'ajouter un feature flag dans la section "**Operation/Feature Manager**" du portail Azure (après avoir sélectionné la **ressource Azure Configuration**) :

![[Pasted image 20240319111920.png]]

![[Pasted image 20240319112033.png]]

Il est ensuite possible de **modifier l'état du flag** directement via le portail Azure.
![[Pasted image 20240319114557.png]]

## Utilisation en ASP.NET Core

Une fois le flag créé, il est possible de récupérer sa valeur au sein du code de la Web application, par exemple pour [[asp-net-core]] :

- **Configurer** le service *AppConfiguration* de l'application de manière à utiliser les feature flags :
	```csharp
	var azureConfigConnectionString = "azure_connection_string_to_get_from_azure_portal";
	
	builder.Host.ConfigureAppConfiguration(builder =>
	{
		builder.AddAzureAppConfiguration(options =>
			options.Connect(azureConfigConnectionString).UseFeatureFlags()
		);
	}
	);
```

- Ajouter le *package Nuget* **Microsoft.FeatureManagement.AspNetCore** à la solution

- Ajouter le service **FeatureManagement** :
	```csharp
	using Microsoft.FeatureManagement;
	//.....
	builder.Services.AddFeatureManagement();
```

- **Injecter** un *FeatureManager* au service voulant consommer le Feature Flag :
	```c#
	using Microsoft.FeatureManagement;

	public class MyService : IMyService
	{
		private readonly IConfiguration _configuration;
		private readonly IFeatureManager _featureManager;

		public MyService(IConfiguration configuration, IFeatureManager featureManager)
		{
			_configuration = configuration;
			_featureManager = featureManager;
		}

		public async Task<bool> GetFlag()
		{
			return await _featureManager.IsEnabledAsync("MyFlagName");
		}
		
	}
```

## Voir aussi

- [[azure-app-configuration]]
- [[azure-web-application]]
