
Le middleware de routing, dans des applications ASP.NetCore, permet de diriger une requête HTTP vers le bon endpoint (qui est aussi un middleware), en fonction de l'URL et du verbe de la requête.

Il doit être configurer en appelant, dans l'ordre : 
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

Il est possible de gérer des [[Routes paramétrées ASP.NetCore|Routes paramétrées]].

La méthode endpoints.MapControllers() permet de configurer les routes via des [[ASP.Net Core Controllers|controleurs]].

**Note** : Le routeur ASP .Net Core détermine le endpoint en suivant des **règles de priorité**, si plusieurs endpoints correspondent à la route : 
