
Le Middleware .Net Core est un composant logiciel intermédiaire qui gère le pipeline de traitement des requêtes HTTP entrantes d'un serveur Web .Net Core.

```csharp
//Program.cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseMyCustomMiddleware(); //Appelle un middleware custom

app.MapGet("/", () => "Hello World!");

app.Run();
```
**Note** : Le middleware app.Run() met un terme au pipeline

Par convention, on définit le middleware en ajoutant une [[Méthodes d'extensions en CSharp|méthode d'extension]] à l'interface IApplicationBuilder : 
```csharp
// Définition du middleware
public class MyCustomMiddleware
{
	private readonly RequestDelegate _next;

	public MyCustomMiddleware(RequestDelegate next)
	{
		_next = next;
	}

	public Task Invoke(HttpContext httpContext)
	{

		return _next(httpContext);
	}
}

// Extension method used to add the middleware to the HTTP request pipeline.
public static class MyCustomMiddlewareExtensions
{
	public static IApplicationBuilder UseMyCustomMiddleware(this IApplicationBuilder builder)
	{
		return builder.UseMiddleware<MyCustomMiddleware>();
	}
}
```

**Note** : L'ordre d'exécution des middlewares est important, chaque middleware va traiter la requête HTTP avant de la passer au middleware suivant.

Les middlewares les plus courants sont, dans l'ordre : 
1. Exception Handler
2. HSTS
3. HttpsRedirection
4. StaticFiles
5. [[Middleware de Routing ASP.NetCore|Routing]]
6. CORS
7. Authentication
8. Authorization
9. ..puis les Middleware Custom

Exemple : 
```csharp
app.UseForwardedHeaders();

app.UseWebOptimizer();

if (_hostingEnvironment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
else
{
    app.UseHsts();
    app.UseHttpsRedirection();
}

app.UseCors();
app.UseContentSecurityPolicyMiddleware();
app.UseRouteNotFoundMiddleware();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthentication();
app.UseJwtTokenMiddleware();
app.UseSwaggerAuthenticationMiddleware();
app.UseAuthorization();
```
