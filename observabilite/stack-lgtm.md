---
tags: [observability, lgtm, grafana, open-source]
---

# Stack LGTM

Ensemble open-source et **agnostique du fournisseur** de Grafana Labs couvrant les 3 piliers de l'[[observabilite-piliers|observabilité]].

## Les composants

| Lettre | Outil | Pilier |
|---|---|---|
| **L** | Loki | Logs |
| **G** | [[grafana\|Grafana]] | Visualisation |
| **T** | [[grafana-tempo\|Tempo]] | Traces |
| **M** | Mimir | Métriques (Prometheus scalable) |

## Positionnement

- Pendant open-source complet des backends propriétaires (App Insights, Datadog…).
- Alimenté via [[opentelemetry|OpenTelemetry]] (apps → [[opentelemetry-collector|Collector]] → fan-out).
- **Self-hosté** (contrôle du coût, ops à porter) ou **Grafana Cloud** (managé).

Choix cohérent quand la **portabilité** et le **multi-cloud/hybride** priment ([[vendor-lock-in]]).

## Voir aussi

- [[grafana-tempo]]
- [[opentelemetry-collector]]
