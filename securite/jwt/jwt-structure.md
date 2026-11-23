---
tags: [securite, jwt, token, oauth]
---

# JWT — Structure d'un JSON Web Token

Un **JWT** (RFC 7519) est un jeton compact et autoportant qui transporte des affirmations (*claims*) sur un sujet. Il est **signé** par un émetteur, ce qui permet à n'importe quel destinataire de vérifier son origine et son intégrité sans avoir à rappeler l'émetteur.

## Syntaxe / Exemple

Trois segments **Base64URL**, séparés par des points :

```text
eyJhbGciOiJSUzI1NiIsImtpZCI6ImFiYzEyMyJ9 . eyJpc3MiOiJodHRwczovL...  . SflKxwRJSMeKKF2QT4fwpM...
└──────────── header ──────────────────┘   └──── payload ─────────┘   └──── signature ──────┘
```

Une fois décodés :

```json
// Header : comment le jeton est signé
{ "typ": "JWT", "alg": "RS256", "kid": "abc123" }

// Payload : les claims
{ "iss": "https://login.microsoftonline.com/<tenant>/v2.0",
  "aud": "<client-id-de-l-api>",
  "sub": "<id-de-l-appelant>",
  "exp": 1764000000,
  "roles": ["Api.Call"] }
```

Le troisième segment est la **signature** de `base64url(header) + "." + base64url(payload)`.

## Détails

- **Header** :
  - `alg` donne l'algorithme de signature ;
  - `kid` (*Key ID*) indique quelle clé publique utiliser pour vérifier la signature ;
  - `typ` donne le type de jeton.
- **Payload** : un objet JSON de claims. Voir [[jwt-claims]].
- **Signature** : elle garantit l'**intégrité** (une modification d'un seul octet invalide le jeton) et l'**authenticité** (seul le détenteur de la clé privée a pu produire la signature). Voir [[jwt-signature-jwks]].
- **Base64URL** : variante de Base64 sans `+`, `/` ni `=` de padding, utilisable telle quelle dans une URL ou un header HTTP.

### Signé ≠ chiffré

C'est le point le plus important à retenir : le payload est simplement **encodé**, donc **lisible par n'importe qui** (jwt.ms, jwt.io, `base64 -d`).

- Ne jamais mettre de secret ni de donnée sensible dans un JWT.
- Un JWT signé s'appelle un **JWS**. Il existe aussi des JWT chiffrés (**JWE**), mais ils sont rares pour les access tokens.

### Jeton « porteur » (*bearer*)

Un access token JWT est un **bearer token** : celui qui le détient peut l'utiliser, sans autre preuve d'identité.

- Il circule dans le header `Authorization: Bearer <jwt>`.
- Il ne doit transiter **qu'en HTTPS**.
- Il ne doit **jamais apparaître dans les logs**.
- On lui donne une **durée de vie courte**.

Pour aller plus loin : les jetons liés à leur émetteur (*sender-constrained*), via DPoP ou mTLS, empêchent la réutilisation d'un jeton volé.

## Voir aussi

- [[jwt-claims]]
- [[jwt-signature-jwks]]
- [[jwt-decoder]]
- [[oauth-2-0-openid]]
