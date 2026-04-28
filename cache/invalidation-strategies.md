---
tags: [cache, invalidation, architecture]
---

# Invalidation de cache — Vue d'ensemble

L'invalidation de cache est le problème le plus difficile du cache : quand et comment expulser des entrées devenues périmées.

## Les grandes stratégies

| Stratégie | Principe | APIM built-in | Redis | Autre |
|-----------|----------|:---:|:---:|-------|
| [[invalidation-ttl\|TTL]] | Expiration automatique après un délai | ✅ | ✅ | ✅ CDN, Varnish, Nginx |
| [[invalidation-explicit\|Suppression explicite]] | Supprimer la clé exacte à l'écriture | ⚠️ | ✅ | ✅ Varnish ban, Cloudflare API |
| [[invalidation-versioning\|Versioning de clé]] | Changer la clé = rendre l'ancienne inaccessible | ✅ | ✅ | ✅ partout |
| [[invalidation-tags\|Tags / Surrogate keys]] | Invalider un groupe d'entrées par tag | ❌ | ⚠️ simulé | ✅ Varnish, Fastly, Cloudflare |
| [[invalidation-event-driven\|Event-driven]] | Invalider sur événement backend (pub/sub, queue) | ❌ | ✅ pub/sub | ✅ Service Bus, Event Grid |
| [[invalidation-etag\|ETag / Conditionnel]] | Le client envoie un ETag, le serveur valide | ⚠️ manuel | ⚠️ manuel | ✅ natif HTTP |

## Légende

- ✅ Supporté nativement
- ⚠️ Implémentable manuellement avec effort
- ❌ Non supporté

## Choisir sa stratégie

- **Données rarement mises à jour** → TTL long suffit.
- **Données fréquemment mises à jour, lecture critique** → Event-driven ou suppression explicite.
- **Contenu statique / frontend** → Versioning de clé (cache busting).
- **Ressources liées (ex : toutes les pages d'un produit)** → Tags / Surrogate keys.
- **APIs REST avec clients hétérogènes** → ETag.

## Voir aussi

- [[apim-caching]] — Cache APIM : modes interne vs externe
- [[apim-cache-policies]] — Politiques APIM : cache-lookup, cache-store, cache-remove-value
