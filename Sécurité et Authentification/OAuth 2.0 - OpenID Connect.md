
## OAuth 2.0

[OAuth 2.0](https://oauth.net/2/) est un **protocole** d'**autorisation** qui permet à une application (**client**) d'obtenir un accès limité aux ressources d’un autre service, en **déléguant** l'authentification et l'autorisation à un **serveur d'autorisation** externe (ex : Microsoft Entra, Google, Facebook, etc...).

L'objectif du protocole est l'**obtention** par l'application d'un **Access Token (jeton d'accès)** permettant à l'API détenant la ressource de valider l'accès.

L'application cliente doit préalablement avoir été [[OAuth OpenID Connect - Application Registration|enregistrée]] auprès du service d'autorisation, pour pouvoir interagir avec le serveur d'autorisation.

La méthode d'obtention de l'access token dépend du **Grant type** utilisé :
- *Client Credential* : utilisé pour les applications **serveur-à-serveur** où il n'y a **pas d'utilisateur final** impliqué
- [[OAuth 2 - Authorization Code|Authorization Code]] : utilisé pour accorder un accès **au nom d'un utilisateur final**, via un [[Clients Publics et Privés#Clients privés|client privé]].
- *PKCE* **(Proof Key for Code Exchange)** : **Extension** de l'**Autorization Code** grant type, pour **renforcer** la **sécurité** lors de l'échange du code d'autorisation pour un jeton d'accès (notamment via un [[Clients Publics et Privés#Clients publics|client public]].)
- *Device Code* : utilisé dans le cas de client n'ayant pas d'interface graphique (IoT)
- *Refresh token* : permet d'obtenir un nouveau jeton d'accès, après expiration du précédent.

## OpenID Connect

[OpenID Connect](https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc?WT.mc_id=azureportalcard_Service_AAD_-inproduct-azureportal) est une **extension** de **OAuth** 2.0 qui permet l'authentification de l'utilisateur en plus de l'autorisation.
Il permet à une application de **déléguer l'authentification** à un **fournisseur OpenID**, qui renvoie un **jeton d'identité (ID Token)** à l'application après l'authentification de l'utilisateur.

Exemple de workflow, via le serveur d'authentification Microsoft Entra : 

![[Pasted image 20240827095354.png]]

## Sécurité dans OAuth 2.0 et OpenID Connect

- **Confidentialité des communications:** Les communications sont **sécurisées en utilisant HTTPS** pour éviter l'interception ou la modification des données en transit.
- **Enregistrement préalable du client** : Les applications tierces (les "clients") doivent s'enregistrer auprès du fournisseur d'authentification (le "serveur d'autorisation" dans le cas d'OAuth 2.0, ou le "fournisseur OpenID" dans le cas d'OpenID Connect) avant de pouvoir utiliser le service. Lors de l'enregistrement, l'application reçoit un **identifiant client et un secret client**, qu'elle doit utiliser pour s'authentifier auprès du fournisseur d'authentification.
- **Redirections sécurisées:** Le fournisseur d'authentification utilise une **URL de redirection pré-enregistrée** (lors de l’enregistrement du client) pour renvoyer l'utilisateur à l'application.
- **Protection contre les attaques de type CSRF:** L'application envoie une valeur "state" unique lors de la demande d'autorisation, et le fournisseur d'authentification la renvoie avec le code d'autorisation.
- **Protection des jetons:** Les jetons d'accès, les codes d'autorisation, et les jetons d'identité sont conçus pour être difficiles à deviner et à falsifier, et leur utilisation est limitée dans le temps.