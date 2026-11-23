---
tags: [index, moc, securite, jwt]
---

# JWT & jetons OAuth — Map of Content

Parcours conseillé : structure → claims → signature → audience/issuer → validation → pièges.

## Fondamentaux

- [[jwt-structure]] — header.payload.signature, Base64URL, signé ≠ chiffré, bearer token
- [[jwt-claims]] — claims standards (`iss`, `aud`, `sub`, `exp`…) et d'autorisation (`scp`, `roles`)
- [[jwt-signature-jwks]] — RS256 vs HS256, `kid`, JWKS, document de découverte OpenID, rotation des clés
- [[jwt-audience-issuer]] — à qui est destiné le jeton, qui l'a émis ; analogie du chèque, les 4 acteurs (`iss`/`azp`/`sub`/`aud`)
- [[jwt-audience-attaques]] — *confused deputy*, rôle qui change de sens, pourquoi `aud` n'est pas une autorisation

## Validation et sécurité

- [[jwt-validation-checklist]] — les 7 contrôles d'une API, dans l'ordre, et le code HTTP associé
- [[authentification-vs-autorisation]] — AuthN vs AuthZ, 401 vs 403, le piège du « rôle oublié »
- [[jwt-pieges-securite]] — `alg:none`, confusion d'algorithme, révocation, fuite, stockage

## Jetons et flux OAuth

- [[access-token-vs-id-token]] — access, ID et refresh tokens : chacun son destinataire
- [[oauth-grant-types]] — Authorization Code + PKCE, Client Credentials, OBO, `/.default`

## Pratique

- [[jwt-decoder]] — obtenir un jeton avec `az`, le décoder en Bash ou PowerShell, diagnostiquer un 401

## Côté Azure

- [[entra-access-token-claims]] — `tid`, `oid`, `azp`/`appid`, `idtyp`…
- [[entra-access-token-versions]] — jetons v1 vs v2, `accessTokenAcceptedVersion`
- [[entra-scopes-vs-app-roles]] — `scp` vs `roles`, définir et accorder un app role
- [[apim-validate-jwt]] — la policy APIM qui applique la checklist
