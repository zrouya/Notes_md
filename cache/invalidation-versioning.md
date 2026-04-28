---
tags: [cache, invalidation, versioning]
---

# Invalidation par versioning de clé (Cache Busting)

Au lieu de supprimer une entrée, on change la clé de cache. L'ancienne entrée devient inaccessible et expire naturellement via TTL. Très utilisé pour les assets frontend.

## Principe

```
# v1 : clé = "products-list-v1"
# Après mise à jour : clé = "products-list-v2"
# L'ancienne clé est abandonnée, pas supprimée
```

## Deux variantes

### Versioning dans l'URL (frontend / CDN)
```html
<script src="/app.js?v=abc123"></script>
<!-- ou -->
<script src="/app.abc123.js"></script>
```
Le hash change à chaque build → le CDN/navigateur traite comme une nouvelle ressource.

### Versioning dans la clé de cache (API / Redis)
```python
version = db.get("products_version")  # ex: "42"
key = f"products-list-v{version}"
# Pour invalider : db.increment("products_version")
# L'ancienne clé expire via TTL
```

## Avantages

- **Pas de suppression** : pas de race condition.
- Fonctionne sur n'importe quel cache (CDN, Redis, APIM...).
- Rollback facile : revenir à l'ancienne version = changer la clé.

## Inconvénients

- Les anciennes entrées restent en mémoire jusqu'à expiration (TTL) → gaspillage mémoire.
- La version doit être accessible au point de lecture **et** d'écriture.
- Moins adapté aux objets individuels (1000 produits = 1000 versions à gérer).

## Implémentation

### APIM built-in
```xml
<!-- vary-by-query-parameter avec un paramètre de version -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
  <vary-by-query-parameter>v</vary-by-query-parameter>
</cache-lookup>
```
Le client passe `?v=42` — APIM voit une clé différente à chaque version.

### Redis
```bash
SET "products-list-v42" value EX 3600
# Invalider = incrémenter la version dans un autre key
INCR products_version
```

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[invalidation-explicit]] — Alternative : suppression directe de la clé
