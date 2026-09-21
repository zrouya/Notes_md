
Les vues (Razor views) dans le framework ASP .Net Core MVC permettent le rendu HTML côté serveur. Elles dérivent  de la classe RazorBasePage.
Elles permettent d'implémenter de la logique de présentation au sein d'une page web, via l'intégration de code C# dans la page HTML (-> fichier .cshtml) : 

```razor
@*
    Commentaires
*@
@using MyEmptyApp.Models;
@{
    // Code C# au sein de la vue
    string appTitle = "Titre de la page";
    var myUser = new User("userName", "UserEmail", new List<string>() { "SomeRole", "Admin" });
}
<!DOCTYPE html>
<html>
    <head>
        <title>@appTitle</title> @* Expression *@
        <meta charset="utf-8" />
    </head>
    <body>
        Welcome to the page @myUser.Name @* Expression *@
        @if (myUser.Name == "someName") @* Bloc conditionnel *@
        {
            <span>...</span>
        }
        @foreach (var role in myUser.Roles) @* Bloc itératif *@
        {
            <section>Role</section>
        }
    </body>
</html>
```

## Liens entre Controllers et Vues

Par défaut, ASP .Net Core associe [[ASP.Net Core Controllers|Controllers]] et Vues via le système de fichier.
Exemple : Pour cette action de Controller :
```csharp
public class AccountController : Controller
{
	public IActionResult SignIn(string? eMail, string? passWord)
	{
		return View();
	}
}
```
.Net Core va chercher un fichier ``SignIn.cshtml`` dans les dossiers :
- ``/Views/Account/``
- Si la vue n'y est pas trouvée, .Net Core va chercher ``/Views/Shared/SignIn.cshtml``.

Il est cependant possible de spécifier un chemin vers une vue spécifique : 
```csharp
    public class AccountController : Controller
    {
        public IActionResult SignIn(string? eMail, string? passWord)
        {
            return View("Views/otherPath");
        }
```

## Echanges de données Controller/Vue

1. ViewState / ViewBag
	Le Controller partage avec sa Vue l'objet ``ViewState``, qui est un **dictionnaire** permettant d'échanger les données dans les 2 sens.
	Pour éviter d'avoir à caster les valeurs du dictionnaire à chaque utilisation, il est possible d'utiliser l'objet **dynamic** ``ViewBag``, qui traduit le dictionnaire ViewState en objet.
	
2. ViewModel
	Il est possible de définir un **objet fortement typé** pour échanger des données entre Controller et Vue : 
```razor
@model User
@* Code de la vue *@
```
