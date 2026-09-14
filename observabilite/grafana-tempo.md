---
tags: [observability, grafana, tempo, tracing, lgtm]
---

# Grafana Tempo

**Backend de tracing distribué** open-source (Grafana Labs) : le **magasin de traces**, agnostique du fournisseur.

## Fonctionnement

```
Apps / Collector ──OTLP──► Tempo ──► stockage objet (Azure Blob / S3)
                                          │
                                       Grafana ──TraceQL──► waterfall
```

- **Ingestion** : spans en **OTLP** (protocole [[opentelemetry|OpenTelemetry]] natif).
- **Stockage** : gros volumes à **bas coût** sur stockage objet, index minimal (`trace_id` + **TraceQL**).
- **Restitution** : [[grafana|Grafana]] rend le waterfall, corrélation vers logs/métriques via le `trace_id`.

## vs Application Insights

| | [[appinsights-principe\|App Insights]] | Tempo |
|---|---|---|
| Nature | APM managé Azure | Backend open-source (self-hosté / Grafana Cloud) |
| Ingestion | SDK/OTel → Azure | **OTLP natif** |
| Coût | au Go | **stockage objet bon marché** |
| Lock-in | Azure | **neutre** |

Même instrumentation OTel dans les deux cas : changer de backend ne touche pas le code ([[vendor-lock-in]]).

## Voir aussi

- [[stack-lgtm]]
- [[opentelemetry-collector]]
- [[correlation-traces-distribuees]]
