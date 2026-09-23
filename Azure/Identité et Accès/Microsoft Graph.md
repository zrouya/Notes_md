
Microsoft Graph est l'**API Azure** permettant d'accéder aux utilisateurs associés à un compte [[Microsoft Entra ID (ex. Azure Active Directory)|Microsoft Entra]].

Pour permettre à une application d'accéder à l'API Graph, et gérer les autorisations d'accès associées, il convient : 
- De créer un [[Azure Application Object|application object]] Azure pour l'application
- De configurer les **API permissions** de cet application object selon : 
	- Que l'application s'authentifie [au nom d'un utilisateur](https://learn.microsoft.com/en-us/graph/auth-v2-user?tabs=http)
	- Ou qu'elle s'authentifie [en son nom](https://learn.microsoft.com/en-us/graph/auth-v2-service?tabs=http)

