
Le Model Binding est une fonctionnalité de .NetCore MVC qui permet de **récupérer les données** des **requêtes HTTP** entrantes (routedata, body, ...) et de les **injecter** en tant que paramètres dans les actions des classes [[ASP.Net Core Controllers|Controllers]].

Les sources de données sont **priorisées** de la manière suivante : 
![[Pasted image 20240617132927.png]]

- **Form fields** : 
	Les données sont récupérées via le corps de la requête, dans le cas ou le Header HTTP Content type vaut ``Content-Type: application/x-www-form-urlencoded`` ou ``Content-Type: multipart/form-data``
- **Request body** : 
	Les données sont récupérée via le corps de la requête, dans le cas ou le Header HTTP Content-type vaut ``Content-Type: application/json`` ou ``Content-Type: application/xml``
- **Route Data** :
	Les données sont récupérées via les [[Routes paramétrées ASP.NetCore|routes paramétrées]]
- **Query string** : 
	Les données sont récupérées via les paramètres dans l'url : ``url/?param1=value?&param2=value2``

Il est possible de forcer la source d'une données à l'aide des attributs [FromBody], [FromRoute], [FromQuery] (attribut sur un paramètre, une méthode de controller, ou une propritété de classe Model).
