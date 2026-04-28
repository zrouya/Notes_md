---
tags: [cache, invalidation, event-driven, pub-sub]
---

# Invalidation par événements (Event-driven)

Le backend publie un événement quand une donnée change. Un consommateur écoute et invalide le cache en conséquence. Découple l'invalidation du chemin d'écriture.

## Principe

```
backend → [écrit en DB] → publie event "product.updated" { id: 42 }
                                          ↓
                              cache-invalidator écoute
                                          ↓
                              cache.delete("product:42")
```

## Avantages

- **Découplage fort** : le backend ne sait pas qu'il y a un cache.
- Invalide en temps quasi-réel sans TTL court.
- Supporte des topologies complexes (plusieurs caches, plusieurs consumers).

## Inconvénients

- Infrastructure supplémentaire requise (broker de messages).
- Garantie de livraison at-least-once → risque de double invalidation (gérable).
- At-most-once (si le broker échoue) → cache périmé jusqu'au TTL fallback.
- Plus difficile à tester et à opérer.

## Implémentation

### APIM built-in
❌ APIM n'a pas de listener d'événements natif.
Contournement : une Azure Function consomme les événements et appelle une policy APIM via l'API de gestion pour vider le cache (complexe, rarement fait).

### Redis Pub/Sub
```bash
# Publisher (backend)
PUBLISH cache-invalidation '{"key": "product:42"}'

# Subscriber (cache-invalidator service)
SUBSCRIBE cache-invalidation
# → reçoit le message, exécute DEL product:42
```

### Redis Keyspace Notifications (auto-invalidation)
```bash
# Activer les notifications d'expiration
CONFIG SET notify-keyspace-events Ex

# Un subscriber écoute les expirations pour propager vers d'autres caches
SUBSCRIBE __keyevent@0__:expired
```

### Azure — Event-driven
```
DB change → Azure Service Bus / Event Grid
             → Azure Function (consumer)
             → DEL sur Redis
             → (optionnel) Purge CDN via API
```

### Kafka / RabbitMQ (générique)
Même pattern : topic `cache.invalidation`, consumer dédié, DEL sur Redis ou purge CDN.

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[invalidation-tags]] — Combiner avec des tags pour l'invalidation par groupe
- [[invalidation-explicit]] — Version synchrone (même chemin que l'écriture)
