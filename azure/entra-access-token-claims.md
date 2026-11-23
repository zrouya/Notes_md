---
tags: [azure, entra, jwt, token, claims]
---

# Entra ID — Claims d'un access token

En plus des claims standards (`iss`, `aud`, `exp`…), Entra ID ajoute ses propres claims, qui identifient le tenant, l'appelant et l'application cliente.

## Syntaxe / Exemple — jeton v2 obtenu en *client credentials*

```json
{
  "aud": "11111111-....",              // client ID de l'API (jeton v2)
  "iss": "https://login.microsoftonline.com/<tid>/v2.0",
  "iat": 1764000000, "nbf": 1764000000, "exp": 1764003900,
  "azp": "22222222-....",              // client ID de l'application appelante
  "azpacr": "1",                       // 0 = client public, 1 = secret, 2 = certificat
  "idtyp": "app",                      // jeton applicatif (claim optionnel)
  "oid": "33333333-....",              // objet du service principal appelant
  "sub": "33333333-....",
  "tid": "<tenant-id>",
  "roles": ["Api.Call"],               // app roles accordés à l'appelant
  "ver": "2.0"
}
```

## Les claims Entra ID utiles

| Claim | Signification |
|---|---|
| `tid` | Tenant de l'appelant. À contrôler sur une API multi-tenant |
| `oid` | ID d'objet **stable** de l'utilisateur ou du service principal. C'est la bonne clé d'identité |
| `sub` | ID propre au couple (utilisateur, application) : sa valeur change d'une application à l'autre |
| `azp` (v2) / `appid` (v1) | Client ID de **l'application qui a demandé** le jeton |
| `scp` | Scopes délégués, séparés par des espaces : `"Data.Read Data.Write"` |
| `roles` | App roles : tableau JSON |
| `idtyp` | `app` ou `user`, si le claim optionnel est activé |
| `wids` | Rôles d'annuaire de l'utilisateur. Voir [[entra-directory-roles]] |
| `groups` | Groupes, si configurés dans *Token configuration* (attention à l'overage au-delà de 200 groupes) |
| `ver` | Version du jeton : `1.0` ou `2.0` |
| `upn` / `preferred_username` | Identifiant lisible de l'utilisateur (absent d'un jeton applicatif) |

## Détails

- **Distinguer un jeton applicatif d'un jeton utilisateur** : présence de `scp` → délégué (un utilisateur est derrière). Seulement `roles`, sans `scp` → applicatif. `idtyp` permet de le savoir de façon explicite.
- **Ne jamais utiliser `upn` ou `email` comme identifiant** : ces valeurs sont modifiables. Utiliser `oid` + `tid`.
- **Claims optionnels** : ils s'ajoutent dans l'App Registration de l'**API** (*Token configuration*), car c'est la ressource qui définit le contenu de ses jetons.

## Voir aussi

- [[jwt-claims]]
- [[entra-access-token-versions]]
- [[entra-scopes-vs-app-roles]]
- [[jwt-decoder]]
