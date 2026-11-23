---
tags: [securite, jwt, cryptographie, openid, jwks]
---

# JWT — Signature, JWKS et découverte OpenID

Comment le destinataire d'un JWT vérifie-t-il la signature sans partager de secret avec l'émetteur ? Grâce à la **cryptographie asymétrique** et à la **publication des clés publiques** de l'émetteur.

## Syntaxe / Exemple

Document de découverte OpenID Connect (*discovery*) :

```bash
curl -s https://login.microsoftonline.com/<tenant>/v2.0/.well-known/openid-configuration \
  | jq '{issuer, jwks_uri}'
# {
#   "issuer":   "https://login.microsoftonline.com/<tenant>/v2.0",
#   "jwks_uri": "https://login.microsoftonline.com/<tenant>/discovery/v2.0/keys"
# }
```

Le JWKS (*JSON Web Key Set*) liste les clés publiques en cours de validité :

```json
{ "keys": [ { "kty": "RSA", "use": "sig", "kid": "abc123", "n": "...", "e": "AQAB" } ] }
```

## Familles d'algorithmes

| `alg` | Type | Signature produite avec | Vérification faite avec |
|---|---|---|---|
| `HS256` | Symétrique (HMAC) | Secret partagé | **Le même secret** |
| `RS256` | Asymétrique (RSA) | Clé privée de l'émetteur | Clé publique (JWKS) |
| `ES256` | Asymétrique (courbe elliptique) | Clé privée | Clé publique |

Les fournisseurs d'identité (Entra ID, Auth0, Keycloak…) utilisent un algorithme **asymétrique**, le plus souvent `RS256`. Ainsi, toute API peut vérifier un jeton sans jamais avoir le pouvoir d'en émettre un.

## Vérifier une signature, pas à pas

1. Lire le `kid` dans le header du jeton.
2. Récupérer le JWKS à partir du `jwks_uri` du document de découverte (en pratique, mis en cache).
3. Choisir la clé dont le `kid` correspond.
4. Recalculer la signature de `header.payload` et la comparer au troisième segment.

## Détails

- **Rotation des clés** : l'émetteur change régulièrement ses clés et publie temporairement l'ancienne et la nouvelle dans le JWKS.
  - Il ne faut **jamais coder une clé en dur** : on pointe vers la *metadata URL*.
  - Les bibliothèques et APIM rafraîchissent le JWKS périodiquement.
- **Ce que fournit le document de découverte** : l'`issuer` attendu **et** les clés publiques. C'est pourquoi `openid-config` suffit à configurer [[apim-validate-jwt]].
- **Ne pas faire confiance à l'`alg` du header** : le validateur doit imposer la liste des algorithmes qu'il accepte. Voir [[jwt-pieges-securite]].
- **Cas particulier Entra ID** : les jetons dont l'audience est Microsoft Graph contiennent un `nonce` dans le header. Leur signature ne peut pas être vérifiée par un tiers. C'est normal : ces jetons ne vous sont pas destinés.

## Voir aussi

- [[jwt-structure]]
- [[jwt-validation-checklist]]
- [[oauth-2-0-openid]]
