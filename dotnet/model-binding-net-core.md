---
tags: [dotnet, aspnet-core, mvc]
---

# Model Binding — .NET Core

Le Model Binding est une fonctionnalité de .NET Core MVC qui permet de **récupérer les données** des **requêtes HTTP** entrantes (routedata, body, ...) et de les **injecter** en tant que paramètres dans les actions des classes [[asp-net-core-controllers|Controllers]].

Les sources de données sont **priorisées** de la manière suivante :

- **Form fields** : les données sont récupérées via le corps de la requête, dans le cas où le Header HTTP Content-Type vaut `Content-Type: application/x-www-form-urlencoded` ou `Content-Type: multipart/form-data`.
- **Request body** : les données sont récupérées via le corps de la requête, dans le cas où le Header HTTP Content-Type vaut `Content-Type: application/json` ou `Content-Type: application/xml`.
- **Route Data** : les données sont récupérées via les [[routes-parametrees-asp-net-core|routes paramétrées]].
- **Query string** : les données sont récupérées via les paramètres dans l'url : `url/?param1=value?&param2=value2`.

Il est possible de forcer la source d'une donnée à l'aide des attributs `[FromBody]`, `[FromRoute]`, `[FromQuery]` (attribut sur un paramètre, une méthode de controller, ou une propriété de classe Model).

## Voir aussi

- [[asp-net-core-controllers]]
- [[model-validation-net-core]]
- [[routes-parametrees-asp-net-core]]
