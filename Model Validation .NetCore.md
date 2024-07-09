
ASP .Net Core MVC fournit une logique de validation des données transmises aux Controllers par le [[Model Binding .NetCore|model binding]].

On peut ajouter des attributs de validation aux propriétés des classes Model qui sont passées aux actions des Controllers : 

```csharp
using System.ComponentModel.DataAnnotations; // namespace des attributs

public class User
{
	[Required(ErrorMessage = "Id must be supplied")]
	public int Id {get; set;}

	[StringLength(int maximumLength, MinimumLength = value, ErrorMessage = "{0} is not from the good length")] // le message est formatté, par défaut {0} correspond au nom de la propriété
	[Display("User name")] // Remplace le nom de la propriété dans le message d'erreur retourné
	public string Name {get; set; }
	[Range(int minimum, int maximum, ErrorMessage = "value")] // pour les valeurs numériques
	[RegularExpression(string pattern, ErrorMessage = "value")]
	[EmailAddress(ErrorMessage = "value")]
	[Phone(ErrorMessage = "value")]
	[Compare(string otherProperty, ErrorMessage = "{0} and {1} properties must match")] // Vérifie si 2 propriétés sont égales ou non (par ex: Password et ConfirmPassword)
	// {1} est remplacé par le display name de la seconde propriété comparée
	[Url(ErrorMessage = "value")]
	[ValidateNever] // Exclut la propriété de la validation
}
```

La classe Controller possède une propriété ``ModelState`` qui représente l'état de validation du modèle : 
```csharp
public class UserAccountController : Controller
{
	public IActionResult Login(UserModel user)
	{
		if(!ModelState.IsValid) // Etat de validation globale du model
		{
			List<string> errorList = new List<string>();
			foreach(var value in ModelState.Values) // On boucle sur toutes les propriétés du model
			{
				foreach(var error in value.Errors) // On boucle sur les erreurs de validation de la propriété
				{
					errorList.add(error.ErrorMessage);
				}
			}
		var errorMessages = string.join("/n", errorList);
		return BadRequest(errorMessages);
		}
	}
}
```

Il est possible de créer des attributs de validation custom : 
```csharp
using System.ComponentModel.DataAnnotations;
using System.Reflection;

public CustomValidationAttribute : ValidationAttribute
{
	// Si l'attribut dépend d'une autre propriété (comme [Compare])
	public String OtherPropertyName { get; set; }
	public CustomValidationAttribute(string otherPropertyName)
	{
		this.OtherPropertyName = otherPropertyName;
	}
	
	public override ValidationResult? IsValid(object? value, ValidationContext validationContext)
	{
		// Utiliser la reflection pour accéder à la valeur de l'autre propriété. On peut en effet accéder au type de l'objet Model :
		PropertyInfo? otherProperty = validation.ObjectType.GetProperty(OtherPropertyName);
		// puis à la valeur de l'autre propriété : 
		var otherValue = otherProperty.GetValue(validationContext.ObjectInstance);

		// Test à faire
		// .......
		
		// Validation OK
		return ValidationResult.Success;
		// Validation KO
		return new ValidationResult("ErrorMessage", new string[]{
			// Liste des noms des propriétés invalidées
			validationContext.MemberName, // Propriété courante -> évite les magic strings			
		});
	}
}
```

Pour des validations qui n'ont pas vocation à être réutilisées, il peut être plus simple d'implémenter la logique directement dans l'objet Model, en implémentant l'interface ``IValidatableObject``.

```csharp
public class User : IValidatableObject
{
	public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
	{
		....
	}
}
```