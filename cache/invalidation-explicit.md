---
tags: [cache, invalidation]
---

# Invalidation explicite par clé

Quand une donnée est modifiée, l'application supprime immédiatement la clé de cache correspondante. La prochaine lecture provoque un miss et recharge la donnée fraîche.

## Principe

```
# À l'écriture :
db.update(id=42, data=...)
cache.delete(f"product:{42}")
```

## Avantages

- Invalidation immédiate, sans attendre un TTL.
- Précis : seule la clé concernée est supprimée.

## Inconvénients

- **Nécessite de connaître la clé exacte** au moment de l'écriture.
- Couplage fort entre le producteur (backend) et le cache.
- Race condition possible : delete puis re-SET concurrent avant que le client relise.
- Ne passe pas à l'échelle si une entité est référencée par de nombreuses clés différentes.

## Implémentation

### APIM built-in
```xml
<!-- Section inbound ou backend -->
<cache-remove-value key="@("product:" + context.Request.MatchedParameters["id"])" />
```
Limitation : APIM doit être le point d'entrée de l'écriture pour déclencher la suppression.

### Redis
```bash
DEL product:42             # synchrone
UNLINK product:42          # asynchrone (non-bloquant, préféré en prod)
```

### Varnish / Cloudflare / Fastly
Purge par URL ou clé via API REST (ex : `PURGE /products/42`).

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[invalidation-tags]] — Pour invalider plusieurs clés liées d'un coup
- [[invalidation-event-driven]] — Pour découpler l'invalidation du chemin d'écriture
