---
tags: [azure, log-analytics, cost, monitoring]
---

# Coût de Log Analytics

[[log-analytics|Log Analytics]] se facture principalement à l'**ingestion**, au **Go**. Poste variable à surveiller dans une infra d'observabilité Azure.

## Les postes

- **Ingestion** : ~2–2,5 €/Go (tier *Analytics*, pay-as-you-go, à confirmer selon région/accord). Volume ≈ nb d'événements × taille (~1,5–2 Ko/requête loggée).
- **Rétention** : **31 jours inclus**, puis ~0,10 €/Go/mois au-delà.
- **Métriques vers le workspace** : envoyer `AllMetrics` dans le workspace **coûte de l'ingestion** — alors que [[grafana|Grafana]] lit les métriques plateforme **nativement et gratuitement**. → levier : ne router que les logs.

## Ordre de grandeur

| Événements/mois | ~Volume | Coût indicatif |
|---|---|---|
| 1 M | ~2 Go | quelques € |
| 10 M | ~20 Go | ~40–50 € |
| 100 M | ~200 Go | → tiers de commitment |

Coût **linéaire** au volume → prévisible.

## Leviers de réduction

- **Sampling** à la source (voir [[cardinalite-metriques]]).
- **Basic Logs** : ~4× moins cher à l'ingestion, mais KQL limité → inadapté aux dashboards RED.
- **Commitment tiers** au-delà de ~100 Go/jour.

## Voir aussi

- [[log-analytics]]
- [[cardinalite-metriques]]
