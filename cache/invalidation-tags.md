---
tags: [cache, invalidation, tags, surrogate-keys]
---

# Invalidation par tags / Surrogate keys

Associer des tags à chaque entrée de cache au moment du stockage, puis invalider toutes les entrées portant un tag donné en une seule opération.

## Principe

```
# Stockage
cache.set("product:42", data, tags=["products", "category:electronics"])

# Invalidation ciblée
cache.invalidate_tag("category:electronics")
# → supprime toutes les entrées portant ce tag
```

## Avantages

- Invalide un **groupe logique** d'entrées sans connaître chaque clé.
- Très utile pour les entités liées : un produit peut apparaître dans plusieurs pages/listes.
- Découple la logique d'invalidation du modèle de clés de cache.

## Inconvénients

- Peu supporté nativement (sauf Varnish, Fastly, Cloudflare Cache Tags).
- Redis nécessite une simulation via des Sets — overhead de maintenance.
- Cohérence éventuelle : l'invalidation peut ne pas être atomique sur un cluster.

## Implémentation

### APIM built-in
❌ Non supporté nativement.

### Redis (simulation via Sets)
```bash
# Stockage : associer la clé à ses tags
SET product:42 value EX 3600
SADD tag:products product:42
SADD tag:category:electronics product:42

# Invalidation par tag
keys = SMEMBERS tag:category:electronics   # ["product:42", ...]
DEL keys                                    # supprimer les entrées
DEL tag:category:electronics               # nettoyer le Set
```
Attention : pas atomique, nécessite un Lua script ou MULTI/EXEC pour être safe.

### Varnish (ban par tag)
```vcl
# À l'écriture, ajouter un en-tête dans la réponse
sub vcl_backend_response {
    set beresp.http.X-Cache-Tags = "products category:electronics";
}
# Invalidation
ban req.http.X-Cache-Tags ~ "category:electronics"
```

### Fastly / Cloudflare
```bash
# Fastly — Surrogate-Key header
Surrogate-Key: products category:electronics

# Purge par tag via API
curl -X POST "https://api.fastly.com/service/XXX/purge/category:electronics"
```

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[invalidation-explicit]] — Alternative : suppression clé par clé
- [[invalidation-event-driven]] — Combiner tags + événements pour l'invalidation distribuée
