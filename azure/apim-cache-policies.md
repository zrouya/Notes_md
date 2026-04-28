---
tags: [azure, apim, cache, policy]
---

# Azure APIM — Politiques de cache

Les politiques de cache s'insèrent dans les sections `<inbound>` et `<outbound>` du policy XML.

## Politiques principales

| Politique | Section | Rôle |
|-----------|---------|------|
| `cache-lookup` | `<inbound>` | Vérifie si une réponse cachée existe avant d'appeler le backend |
| `cache-store` | `<outbound>` | Stocke la réponse du backend dans le cache |
| `cache-lookup-value` | `<inbound>` | Lit une valeur arbitraire dans le cache (hors requête HTTP) |
| `cache-store-value` | `<inbound>` / `<outbound>` | Écrit une valeur arbitraire dans le cache |
| `cache-remove-value` | n'importe où | Supprime une entrée du cache par clé |

## Exemple minimal

```xml
<policies>
  <inbound>
    <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" caching-type="prefer-external">
      <vary-by-query-parameter>version</vary-by-query-parameter>
    </cache-lookup>
  </inbound>
  <outbound>
    <cache-store duration="300" />
  </outbound>
</policies>
```

## Paramètres clés de `cache-lookup`

- `duration` — TTL en secondes (aussi sur `cache-store`).
- `caching-type` — `internal` | `external` | `prefer-external`.
- `vary-by-header` — varie la clé de cache par en-tête HTTP.
- `vary-by-query-parameter` — varie la clé de cache par paramètre de query string.
- `vary-by-developer` — clé par identité développeur (abonnement).
- `vary-by-developer-groups` — clé par groupe de développeurs.

## Invalidation

Pas de mécanisme natif par tag. Options :
1. Laisser expirer le TTL.
2. `cache-remove-value` avec la clé exacte.
3. Supprimer directement dans Redis (cache externe).

## Voir aussi

- [[apim-caching]] — Vue d'ensemble et modes de cache
- [[apim-cache-bonnes-pratiques]] — Bonnes pratiques et limites
