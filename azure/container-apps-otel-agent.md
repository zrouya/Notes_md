---
tags: [azure, container-apps, opentelemetry, otlp, monitoring]
---

# Agent OpenTelemetry managé (Container Apps)

Azure Container Apps fournit un **agent [[opentelemetry|OpenTelemetry]] managé** (un [[opentelemetry-collector|Collector]] opéré par Azure) configuré au **niveau du Container Apps Environment**.

## Granularité : config partagée par l'Environment

**Toutes les apps du même Environment partagent la même config de destinations et de routage.** Pas d'override par app.
→ Pour router différemment deux groupes d'apps, il faut des **Environments séparés**.

Réglable :
- **Destinations** : une App Insights (connection string) + une ou plusieurs destinations **OTLP** nommées (fan-out possible).
- **Routage par signal** : traces / logs / metrics routés **séparément** (au niveau Environment).

Azure **injecte automatiquement** `OTEL_EXPORTER_OTLP_ENDPOINT` dans tous les conteneurs → l'app émet juste de l'OTLP.

## Contraintes de routage

| Signal | App Insights | OTLP |
|---|---|---|
| Traces | ✅ | ✅ |
| Logs | ✅ | ✅ |
| Metrics | ❌ | ✅ **uniquement** |

D'où le pattern : **traces + logs → App Insights**, **metrics → OTLP** (Prometheus/Grafana).

## Limite

Collector volontairement bridé : pas de pipeline custom, pas de tail sampling, pas de transformation avancée. Pour ça → [[opentelemetry-collector|Collector auto-hébergé]] en relais.

## Voir aussi

- [[opentelemetry]]
- [[opentelemetry-collector]]
- [[container-apps-app-logs]] — L'autre canal : logs console/système
- [[appinsights-auto-instrumentation]]
