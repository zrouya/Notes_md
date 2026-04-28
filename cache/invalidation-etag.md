---
tags: [cache, invalidation, etag, http, conditionnel]
---

# Invalidation par ETag / Requêtes conditionnelles HTTP

Le serveur attribue un identifiant opaque (ETag) à chaque version d'une ressource. Le client le renvoie lors des requêtes suivantes ; le serveur répond `304 Not Modified` si la ressource n'a pas changé, sans retransmettre le corps.

## Principe

```
# 1ère requête
GET /products/42
← 200 OK  ETag: "abc123"  [corps complet]

# Requêtes suivantes
GET /products/42  If-None-Match: "abc123"
← 304 Not Modified  [corps vide, cache local toujours valide]
← 200 OK  ETag: "xyz789"  [nouvelle version si changé]
```

## Variante : Last-Modified / If-Modified-Since
Même logique avec une date au lieu d'un hash. Moins précis (résolution à la seconde).

## Avantages

- Standard HTTP, supporté par tous les clients et navigateurs.
- Économise la bande passante (pas de body si non modifié).
- Le serveur reste la source de vérité pour la fraîcheur.

## Inconvénients

- **N'élimine pas la requête réseau** : le client doit toujours appeler le serveur.
- Overhead si le calcul de l'ETag est coûteux (hash du contenu).
- Pas adapté aux caches de type proxy/reverse-proxy autonomes (le cache doit être validé par le backend).
- Complexe à implémenter dans APIM sans logique de validation côté backend.

## Implémentation

### APIM built-in
⚠️ Non natif. Implémentation manuelle via policies :
```xml
<inbound>
  <!-- Lire l'ETag stocké pour cette ressource -->
  <cache-lookup-value key="@("etag:" + context.Request.MatchedParameters["id"])"
                      variable-name="cachedEtag" />
  <!-- Injecter If-None-Match vers le backend -->
  <choose>
    <when condition="@(context.Variables.ContainsKey("cachedEtag"))">
      <set-header name="If-None-Match" exists-action="override">
        <value>@((string)context.Variables["cachedEtag"])</value>
      </set-header>
    </when>
  </choose>
</inbound>
<outbound>
  <!-- Si le backend renvoie un nouvel ETag, le stocker -->
  <choose>
    <when condition="@(context.Response.Headers.ContainsKey("ETag"))">
      <cache-store-value key="@("etag:" + context.Request.MatchedParameters["id"])"
                         value="@(context.Response.Headers["ETag"])" duration="3600" />
    </when>
  </choose>
</outbound>
```

### Redis
```bash
# Stocker l'ETag avec la valeur
SET product:42 '{"data": ..., "etag": "abc123"}' EX 3600
# Ou stocker séparément
SET etag:product:42 "abc123" EX 3600
```

### Nginx / Varnish / CDN
Supporté nativement : transmis et respecté par les en-têtes HTTP sans configuration.

## Voir aussi

- [[invalidation-strategies]] — Vue d'ensemble des stratégies
- [[apim-cache-bonnes-pratiques]] — Approche ETag manuelle dans APIM
- [[invalidation-ttl]] — Complémentaire : TTL comme fallback si le backend est indisponible
