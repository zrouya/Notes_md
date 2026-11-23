---
tags: [azure, apim, jwt, policy, oauth, entra]
---

# APIM — Policy `validate-jwt`

Policy *inbound* d'Azure API Management qui valide un JWT (signature, issuer, expiration, audience, claims requis) **avant** que la requête n'atteigne le backend. Si la validation échoue, APIM répond lui-même avec un 401 et le backend n'est jamais appelé.

## Syntaxe / Exemple

```xml
<validate-jwt header-name="Authorization"
              failed-validation-httpcode="401"
              failed-validation-error-message="Unauthorized. Access token is missing or invalid."
              require-scheme="Bearer"
              clock-skew="60"
              output-token-variable-name="jwt-token">
    <!-- Issuer attendu + clés publiques (JWKS), tirés du document de découverte -->
    <openid-config url="https://login.microsoftonline.com/{{tenant-id}}/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>{{api-client-id}}</audience>   <!-- jetons v2 -->
        <audience>api://mon-api</audience>       <!-- jetons v1, si nécessaire -->
    </audiences>
    <required-claims>
        <claim name="roles" match="any">          <!-- any : au moins une valeur ; all : toutes -->
            <value>Api.Call</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## Correspondance avec la [[jwt-validation-checklist]]

| Élément | Contrôle réalisé |
|---|---|
| `header-name` / `require-scheme` | Le jeton est présent, avec le schéma `Bearer` |
| `openid-config` | Signature (JWKS, avec le `kid`) **et** `iss` |
| `require-expiration-time` (vrai par défaut) | Présence de `exp` |
| `clock-skew` (secondes) | Tolérance appliquée à `exp` et `nbf` |
| `<audiences>` | `aud` doit correspondre à l'une des valeurs |
| `<issuers>` (facultatif) | Liste explicite d'émetteurs, par exemple pour accepter v1 **et** v2 |
| `<required-claims>` | **Autorisation** : `roles`, `scp`, `tid`… |

## Détails

- **`{{...}}`** désigne une *Named Value* APIM : une valeur centralisée, qui peut provenir d'un Key Vault. On évite ainsi de coder le tenant ou le client ID en dur.
- **`output-token-variable-name`** stocke le jeton décodé pour les policies suivantes :

  ```csharp
  @(((Jwt)context.Variables["jwt-token"]).Claims.GetValueOrDefault("oid", ""))
  ```

- **`match="any"` ou `"all"`** : `any` demande qu'au moins une des valeurs listées soit présente, `all` qu'elles le soient toutes.
- **`scp` est une chaîne et non un tableau** : il faut ajouter `separator=" "` sur le `<claim>` pour pouvoir le tester.
- **Le JWKS est mis en cache** et rafraîchi par APIM, ce qui suit automatiquement la rotation des clés.
- **Alternative dédiée à Entra ID** : `validate-azure-ad-token`, qui prend un `tenant-id` et des `<client-application-ids>` (pour restreindre les **applications appelantes**).

## Bonnes pratiques de placement

- **Où placer la policy** :
  - au **niveau global** (*All APIs*) : toutes les API sont protégées d'office, et une API publique doit s'en exclure explicitement ;
  - au **niveau d'une API** : plus souple, mais une API oubliée reste ouverte.
- **Factoriser** : regrouper la validation dans un **policy fragment** et l'inclure avec `<include-fragment fragment-id="..."/>`, ce qui évite de copier-coller un `validate-jwt` qui finit par diverger d'une copie à l'autre.
- **Après validation**, `<set-header name="Authorization" exists-action="delete"/>` : le backend ne reçoit pas un jeton qui ne lui est pas destiné. Il peut s'authentifier autrement, par exemple avec la managed identity d'APIM.
- **Un 401 renvoyé par la policy n'atteint jamais le backend** : le diagnostic se fait dans les traces APIM ou dans `ApiManagementGatewayLogs`. Voir [[apim-gateway-logs]].

## Voir aussi

- [[jwt-validation-checklist]]
- [[entra-access-token-versions]]
- [[entra-scopes-vs-app-roles]]
- [[apim-rate-limiting]]
