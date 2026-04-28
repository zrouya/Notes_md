---
tags: [azure, apim, cache]
---

# Azure APIM — Bonnes pratiques de cache

## Bonnes pratiques

- Toujours définir `vary-by-query-parameter` pour éviter les collisions entre requêtes différentes.
- Utiliser le cache externe (Redis) en production — le cache interne ne survit pas aux redémarrages.
- Ne pas cacher les réponses contenant des données sensibles ou personnalisées sans `vary-by-developer`.
- Penser à la cohérence : APIM ne sait pas si le backend a changé — le cache peut servir des données périmées.
- Choisir un TTL cohérent avec la fréquence de mise à jour des données backend.

## Limites

| Limite | Détail |
|--------|--------|
| Cache interne volatile | Perdu au redémarrage de l'instance APIM |
| Cache externe : latence | Chaque hit passe par Redis (réseau) |
| Pas d'invalidation par tag | Impossible de cibler un groupe d'entrées à invalider |
| Pas de cache conditionnel natif | ETag / If-None-Match à implémenter manuellement via des politiques |

## Cache conditionnel (ETag) — approche manuelle

1. Stocker l'ETag via `cache-store-value` à la première réponse.
2. À chaque requête entrante, lire l'ETag depuis le cache avec `cache-lookup-value`.
3. Ajouter l'en-tête `If-None-Match` vers le backend via `set-header`.
4. En `<outbound>`, intercepter le `304 Not Modified` et servir la réponse cachée.

## Voir aussi

- [[apim-caching]] — Vue d'ensemble et modes de cache
- [[apim-cache-policies]] — Politiques cache-lookup, cache-store, etc.
