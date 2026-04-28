---
tags: [azure, apim, cache]
---

# Azure APIM — Caching

Mécanisme de mise en cache des réponses HTTP dans API Management pour réduire la latence et décharger les backends.

## Deux modes de cache

| Mode | Description | Tiers |
|------|-------------|-------|
| **Interne (built-in)** | Cache en mémoire intégré à APIM | Developer, Premium |
| **Externe (Redis)** | Azure Cache for Redis connecté via une connexion nommée | Tous |

### Cache interne
- Simple à activer, zéro infrastructure supplémentaire.
- Limité en capacité, non partagé entre unités de scale-out.
- **Perdu en cas de redémarrage de l'instance.**

### Cache externe (Redis)
- Recommandé en production : persistance, capacité, partage entre instances.
- Ajoute une dépendance réseau (latence Redis).
- Redis Premium pour la persistance sur disque.

## Voir aussi

- [[apim-cache-policies]] — Politiques cache-lookup, cache-store, etc.
- [[apim-cache-bonnes-pratiques]] — Bonnes pratiques et limites
