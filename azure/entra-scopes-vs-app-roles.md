---
tags: [azure, entra, oauth, jwt, autorisation, app-roles]
---

# Entra ID — Scopes délégués (`scp`) vs App roles (`roles`)

Une API protégée par Entra ID peut exposer deux sortes de permissions. Pour choisir, une question : **l'appel est-il fait au nom d'un utilisateur, ou par une application seule ?**

## Comparaison

| | Scopes délégués | App roles |
|---|---|---|
| Défini dans | *Expose an API* → scopes | *App roles* |
| Claim dans le jeton | `scp` (chaîne séparée par des espaces) | `roles` (tableau) |
| Appelant | Une application **au nom d'un utilisateur** | Une **application seule**, ou un utilisateur/groupe assigné |
| Flux | Authorization Code, OBO, Device Code | Client Credentials (ou connexion utilisateur, pour un rôle utilisateur) |
| Droits effectifs | **Intersection** des droits de l'application et de l'utilisateur | Ceux du rôle, sans contexte utilisateur |
| Octroi | Consentement de l'utilisateur ou de l'admin (`oauth2PermissionGrant`) | **Consentement admin** (`appRoleAssignment`) |
| Nommage typique | `Data.Read` | `Data.Read.All`, `Api.Call` |

## Syntaxe / Exemple — définir un app role (manifeste de l'API)

```json
"appRoles": [{
  "allowedMemberTypes": ["Application"],   // "User" pour un rôle utilisateur, ou les deux
  "displayName": "Appeler l'API",
  "value": "Api.Call",                     // ← valeur qui apparaîtra dans le claim roles
  "id": "<guid>",
  "isEnabled": true
}]
```

## Comment l'appelant obtient le rôle

1. L'App Registration de l'**appelant** déclare la permission *Application* `Api.Call` sur l'API.
2. Un admin **accorde le consentement**, ce qui crée un `appRoleAssignment` sur le service principal de l'appelant.
3. L'appelant demande `scope=api://mon-api/.default` en client credentials.
4. Le jeton contient `"roles": ["Api.Call"]`.

Pour une **managed identity**, il n'y a pas d'App Registration : on crée directement l'`appRoleAssignment` (CLI, PowerShell, Terraform…). Voir [[entra-app-registration-vs-enterprise-app]].

## Détails

- **Un même claim `roles`, deux sens** :
  - un rôle avec `allowedMemberTypes: ["User"]` est assigné à des utilisateurs ou groupes dans l'Enterprise Application, et apparaît dans **leurs** jetons délégués ;
  - un rôle de type `Application` apparaît dans les jetons d'application.
- **Validation côté API** : selon les cas, on exige `roles` (service à service), `scp` (application utilisateur), ou l'un des deux.
- **Même logique que Microsoft Graph** : `User.Read` (délégué) face à `User.Read.All` (application). Voir [[entra-graph-permissions]].

## Voir aussi

- [[authentification-vs-autorisation]]
- [[oauth-grant-types]]
- [[entra-permissions-vs-roles]]
- [[apim-validate-jwt]]
