# Cache — Map of Content

## Invalidation de cache

- [[invalidation-strategies]] — Vue d'ensemble + tableau comparatif APIM / Redis / autres
- [[invalidation-ttl]] — Expiration automatique par TTL
- [[invalidation-explicit]] — Suppression explicite de la clé à l'écriture
- [[invalidation-versioning]] — Changer la clé pour rendre l'ancienne inaccessible (cache busting)
- [[invalidation-tags]] — Invalider un groupe d'entrées par tag / surrogate key
- [[invalidation-event-driven]] — Invalidation via pub/sub ou message queue (découplé)
- [[invalidation-etag]] — Requêtes conditionnelles HTTP (ETag / If-None-Match)

## Azure APIM

- [[apim-caching]] — Modes de cache APIM : interne vs Redis externe
- [[apim-cache-policies]] — Politiques APIM : cache-lookup, cache-store, cache-remove-value
- [[apim-cache-bonnes-pratiques]] — Bonnes pratiques et limites du cache APIM
