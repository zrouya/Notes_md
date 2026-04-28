---
tags: [cache, invalidation, ttl]
---

# Invalidation par TTL (Time-To-Live)

Chaque entrée de cache expire automatiquement après un délai fixé à l'insertion. C'est la stratégie la plus simple et la plus universelle.

## Principe

```
cache.set(key, value, ttl=300)  # expire dans 5 min
```

À l'expiration, la prochaine lecture provoque un cache miss → rechargement depuis le backend.

## Avantages

- Zéro coordination avec le backend.
- Fonctionne partout, sans infrastructure supplémentaire.
- Simple à raisonner : worst-case de péremption = TTL.

## Inconvénients

- Les données peuvent être périmées pendant toute la durée du TTL.
- TTL court = plus de charge backend. TTL long = données plus périmées.
- Pas adapté si la fraîcheur est critique (stock, prix temps réel).

## Implémentation

### APIM built-in / externe
```xml
<cache-store duration="300" />
```

### Redis
```bash
SET key value EX 300       # TTL en secondes
EXPIREAT key 1735689600    # TTL absolu (timestamp Unix)
TTL key                    # vérifier le TTL restant
```

### CDN / Nginx / Varnish
Piloté par les en-têtes HTTP `Cache-Control: max-age=300` ou `s-maxage`.

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[invalidation-explicit]] — Suppression explicite pour un contrôle immédiat
