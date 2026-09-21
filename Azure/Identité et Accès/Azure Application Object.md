
Un Application Object Azure est un artefact permettant d'associer des **autorisation d'accès** à une application, de manière **flexible** et **granulaire**.

Il permet de configurer un **service principal**, un type de [[Security principals|security principal]].

Pour créer un Application Object via le portail Azure, en accédant au menu "**App Registration**" dans la section "Microsoft Entra ID" : 

![[Pasted image 20240814184343.png]]

## Définition des rôles associés

Une fois l'Application Object créé, il est sélectionnable en tant que membre (au même titre que n'importe quel security principal comme un User), d'un [[Azure Role Assignment|role assignement]] (pour un accès à une **[[Resource Azure]]**) :

![[Pasted image 20240814191810.png]]

## API permissions

La gestion d'accès par [[Rôles Azure RBAC (Role Based Access Control)|rôle]] ne concerne que les **ressources** Azure.

Pour qu'une application puisse accéder à une **API** Azure (comme [[Microsoft Graph]]), il faut configurer les **API permissions** de l'application object.

Lors de la création d'une API permission, 2 stratégies d'autorisation sont possibles :
- **Delegate permission** : l'application s'authentifie **au nom d'un utilisateur**, en jouissant donc des autorisations associées à cet utilisateur.
- **Application permission** : l'application s'authentifie **en tant qu'elle même**, sans qu'un compte utilisateur ne soit utilisé.

Le protocole [[OAuth 2.0 - OpenID Connect|OAuth 2]] est alors utilisé pour autoriser l'application à  accéder à l'API, avec les droits associés.

## Creation d'un client secret

Dans la section "**Manage/Certifiates & secrets**", on peut créer un **Client secret**, qui encapsule des **credentials** d'accès de l'**application** aux ressources, selon les rôles spécifiés.

![[Pasted image 20240814185809.png]]

![[Pasted image 20240814185915.png]]

Pour une application .Net, il convient de télécharger le package ``Azure.Identity`` pour utiliser les credentials du client secret : 
```dotnet
dotnet add package Azure.Identity
```

Puis d'utiliser la classe ``ClientSecretCredentials``, qui permet d'accéder aux différentes ressources via les API correspondantes ([[Azure Blob Storage - DotNet applications|blobs]], [[Azure Key Vault|key vaults]], etc...) .
```csharp
using Azure.Identity;

// Identifiant Microsoft Entra (tenant, ou directory)
string tenantId="38dbefc3-d57f-4955-b62c-1406e16a4ea8";
// Identifiant de l'Application object
string clientId="72ee1803-08cf-4c19-a334-8405e09a242c";
// Value du Client Secret créé au sein de l'Application Object
string clientSecret="ywo8Q~iDGv1b1.Uk_CkXbLUw2G4YNLbWUo5s4b1T";
string storageAccountName="appstore55455344243";

ClientSecretCredential clientSecretCredential = new ClientSecretCredential(tenantId,clientId,clientSecret);

// Utilisation de l'objet clientSecretCredential pour accéder aux ressources
// ........