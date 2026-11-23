---
tags: [securite, jwt, api, validation, checklist]
---

# JWT — Checklist de validation d'un access token

Les contrôles qu'une API (ou une passerelle comme APIM) doit faire, **dans cet ordre**, avant de faire confiance à un access token. Un seul échec suffit à rejeter la requête avec un **401**.

## Syntaxe / Exemple

```text
Authorization: Bearer eyJ...      ← 0. Jeton présent, schéma "Bearer"
        │
        ▼
1. Format       3 segments, JSON décodable
2. Algorithme   alg ∈ liste autorisée (ex. RS256) — jamais "none"
3. Signature    vérifiée avec la clé du JWKS désignée par le kid
4. Issuer       iss == émetteur attendu (le bon tenant)
5. Durée        nbf ≤ maintenant < exp (± tolérance d'horloge)
6. Audience     aud == identifiant de MON API
─────────────── à ce stade : jeton authentique et destiné à moi  → AUTHENTIFICATION
7. Autorisation roles / scp contiennent la permission attendue  → AUTORISATION
        │
        ▼
   200 / requête transmise   (échec en 1-6 → 401 ; échec en 7 → 403 en théorie)
```

## Détails

- **Les étapes 3 à 6 sont toutes obligatoires**, car chacune ferme une attaque différente :
  - sans signature, on accepte un jeton forgé ;
  - sans issuer, on accepte un jeton d'un autre tenant ;
  - sans expiration, on accepte un jeton volé il y a longtemps ;
  - sans audience, on accepte un jeton destiné à une autre API.
- **L'étape 7 est trop souvent oubliée** : sans elle, **n'importe quelle application du tenant** capable d'obtenir un jeton pour l'API est acceptée. Voir [[authentification-vs-autorisation]].
- **Code HTTP** : la RFC 6750 prévoit un **403** pour un jeton valide mais sans la bonne permission. Beaucoup de passerelles renvoient pourtant 401 dans tous les cas (APIM `validate-jwt` utilise un seul code par policy).
- **Tolérance d'horloge** (*clock skew*) : quelques minutes au plus, pour absorber les écarts d'horloge entre serveurs.
- **Ne pas réimplémenter ces contrôles** : utiliser une bibliothèque éprouvée (`Microsoft.Identity.Web`, `jose`, `PyJWT`…) ou la policy de la passerelle.
- **Défense en profondeur** : si une passerelle valide le jeton, le backend peut le valider lui aussi, ou ne doit être joignable **que** par la passerelle (réseau privé, filtrage IP, mTLS).

## Voir aussi

- [[jwt-signature-jwks]]
- [[jwt-audience-issuer]]
- [[apim-validate-jwt]]
- [[jwt-pieges-securite]]
