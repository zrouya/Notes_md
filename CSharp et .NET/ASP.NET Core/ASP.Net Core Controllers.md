
Les controllers permettent d'effectuer un [[Middleware de Routing ASP.NetCore|routing]] dynamique au sein d'une application ASP.Net Core.
Pour activer le routing via controllers, la configuration du middleware doit comporter : 

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers(); // Ajoute dynamiquement toutes les classes controllers aux services, en les enregistrant dans l'injecteur de dépendances
var app = builder.Build();

app.UseRouting(); // Configure le service de routage
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers(); // Mappe les endpoints des controllers
});
// Autre manière plus simple de mapper les endpoints : 
app.MapControllers();
```

Le framework .Net Core va répertorier toutes les classes controller (``app.UseRouting()``):
- Toutes les classes dont le nom est suffixé par ``Controller`` (ex : AccountController)
- OU toutes les classes possédant l'attribut ``[Controller]``

Optionnellement, ces classes peuvent hériter de la classe ``Microsoft.AspNetCore.Mvc.Controller``, ce qui leur permet d'accéder à des méthodes facilitant le traitement des requêtes.

Les méthodes de ces classes peuvent, préférentiellement, retourner un objet ``IActionResult``, ou [[ASP.Net Core ActionResult|ActionResult]], qui est la classe mère des principaux types de retours HTTP :

![[Pasted image 20240614154759.png]]

Exemple de controller : 
```csharp
//[Controller]
//Nécessaire si le nom de la classe n'est pas suffixée par 'Controller'
public class AccountController : Controller
{
	public IActionResult SignIn(string? eMail, string? passWord)
	{
		// Do something
		return Content("User is signed in");
	}

	public IActionResult Login(LoginData data)
	{
		//Do something
		return Content("User is logged in");
	}

	public class LoginData
	{
		public string Login { get; set; }
		public string Password { get; set; }
	}
}
```

Les valeurs des paramètres sont injectées par le [[Model Binding .NetCore|model binding]], et validées par la [[Model Validation .NetCore|model validation]].

Note : La route correspondant à l'action d'un controller est configurée par défaut par : ``{Domain}/{ControllerName}/{ActionName}`` 
Il est possible de spécifier une ou plusieurs routes à une action du controller en ajoutant l'attribut **Route** : 
```csharp
[Route("/customurl")]
public IActionResult MyAction() {...}
```

