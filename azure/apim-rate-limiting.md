---
tags: [azure, apim, policy, rate-limit, throttling]
---

# Azure APIM — Rate Limiting & Quotas

Contrôle du débit des appels via des politiques appliquées à différents niveaux de la hiérarchie APIM.

## 4 niveaux de granularité (cumulables)

| Niveau | Portée | Cas d'usage typique |
|--------|--------|---------------------|
| **Global** | Toutes les APIs de l'instance | Filet de sécurité défensif |
| **Product** | Toutes les APIs d'un produit | Quotas par offre (free / premium) |
| **API** | Toutes les opérations d'une API | Limite par service métier |
| **Operation** | Une opération spécifique | Endpoints critiques (`/token`, `/transfer`) |

Les politiques **s'empilent** : une requête traverse global → product → API → operation, chaque niveau peut la rejeter.

## Deux familles de politiques

| Politique | Fenêtre | Par | Comportement |
|-----------|---------|-----|--------------|
| `rate-limit` | Courte (secondes/minutes) | Souscription | Fenêtre glissante, reset auto |
| `rate-limit-by-key` | Courte | Clé arbitraire | Même logique, clé libre |
| `quota` | Longue (jour/mois) | Souscription | Enveloppe cumulative |
| `quota-by-key` | Longue | Clé arbitraire | Même logique, clé libre |

La variante `*-by-key` segmente par IP, claim JWT, header ou expression C# arbitraire.

## Exemple — politiques empilées

```xml
<!-- Global : protection générale -->
<rate-limit calls="1000" renewal-period="60" />

<!-- Niveau API /payments -->
<rate-limit calls="200" renewal-period="60" />

<!-- Niveau opération POST /payments/transfer -->
<rate-limit-by-key calls="10" renewal-period="60"
  counter-key="@(context.Request.Headers.GetValueOrDefault("X-Client-Id"))" />
```

## Points d'attention

- Les limites inférieures doivent être **plus restrictives** que les supérieures, sinon elles n'ont aucun effet pratique.
- `rate-limit` est **par souscription** ; pour limiter par IP/user indépendamment, utiliser `rate-limit-by-key`.
- En **multi-instance ou multi-région** : les compteurs ne sont pas synchronisés par défaut. Option `sync-rate-limit-counters` disponible depuis APIM v2.

## Voir aussi

- [[apim-caching]] — Vue d'ensemble et modes de cache APIM
- [[apim-cache-policies]] — Autres politiques APIM (cache-lookup, cache-store…)
