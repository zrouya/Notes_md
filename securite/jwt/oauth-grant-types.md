---
tags: [securite, oauth, flux, client-credentials, pkce]
---

# OAuth 2.0 — Flux d'obtention de jeton (*grant types*)

Le *grant type* décrit **comment** un client obtient un access token. Il se choisit selon une question simple : **y a-t-il un utilisateur derrière l'appel ?**

## Les flux à connaître

| Flux | Utilisateur ? | Cas d'usage | Claim d'autorisation obtenu |
|---|---|---|---|
| **Authorization Code + PKCE** | Oui | Application web, SPA, application mobile | `scp` (délégué) |
| **Client Credentials** | Non | Service à service, batch, démon | `roles` (application) |
| **On-Behalf-Of (OBO)** | Oui, propagé | Une API qui appelle une autre API pour l'utilisateur | `scp` |
| **Device Code** | Oui | CLI, objet connecté sans navigateur | `scp` |
| ~~Implicit~~, ~~Password (ROPC)~~ | — | **Obsolètes**, à ne plus utiliser | — |

## Syntaxe / Exemple — Client Credentials (Entra ID)

```bash
curl -s -X POST "https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token" \
  -d "grant_type=client_credentials" \
  -d "client_id=<client-id-de-l-appelant>" \
  -d "client_secret=<secret>" \
  -d "scope=api://mon-api/.default" | jq -r .access_token
```

- Le scope `/.default` demande « toutes les permissions **applicatives** déjà accordées » sur cette API. Il est **obligatoire** en client credentials.
- Le jeton obtenu contient `aud` = l'API et `roles` = les app roles accordés à l'appelant.
- Côté Azure, on préfère un **certificat** ou une **managed identity** à un secret client.

## Syntaxe / Exemple — Authorization Code + PKCE (résumé)

1. Le client redirige l'utilisateur vers `/authorize?response_type=code&scope=api://mon-api/Data.Read&code_challenge=...`.
2. L'utilisateur s'authentifie et consent ; le fournisseur d'identité renvoie un `code` sur la `redirect_uri`.
3. Le client échange ce `code` et le `code_verifier` contre des jetons sur `/token`.

Il obtient un access token (avec `scp`), un ID token et un refresh token.

**PKCE** prouve que celui qui échange le code est bien celui qui l'a demandé. Il est obligatoire pour les clients publics (SPA, application mobile) et recommandé partout ailleurs.

## Voir aussi

- [[oauth-2-0-openid]]
- [[entra-scopes-vs-app-roles]]
- [[access-token-vs-id-token]]
- [[managed-identity-comparaison]]
