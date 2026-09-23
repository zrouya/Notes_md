---
tags: [dotnet, aspnet-core, routing]
---

# Middleware de Routing — ASP.NET Core

Le middleware de routing, dans des applications ASP.NET Core, permet de diriger une requête HTTP vers le bon endpoint (qui est aussi un middleware), en fonction de l'URL et du verbe de la requête.

Il doit être configuré en appelant, dans l'ordre :
```csharp
app.UseRouting(); // Configure le service de routage
app.UseEndpoints (endpoints => {
	endpoints.Map("/url", (context) => { //définit le endpoint correspondant à l'url demandée 
		});
	endpoints.MapGet("/getUrl", async (context) => { await ... }); //Spécifie, en plus de l'url, que le verbe HTTP doit être GET (version asynchrone)
	endpoints.MapPost(...); //Pareil, mais pour une requête POST
	endpoints.MapControllers();
});
```

Il est possible de gérer des [[routes-parametrees-asp-net-core|routes paramétrées]].

La méthode `endpoints.MapControllers()` permet de configurer les routes via des [[asp-net-core-controllers|controleurs]].

**Note** : le routeur ASP.NET Core détermine le endpoint en suivant des **règles de priorité**, si plusieurs endpoints correspondent à la route.

## Voir aussi

- [[routes-parametrees-asp-net-core]]
- [[asp-net-core-controllers]]
- [[asp-net-core-middlewares]]
