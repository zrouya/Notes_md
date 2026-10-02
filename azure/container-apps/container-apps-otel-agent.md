---
tags: [azure, container-apps, opentelemetry, otlp, monitoring]
---

# Agent OpenTelemetry managé (Container Apps)

Azure Container Apps fournit un **agent [[opentelemetry|OpenTelemetry]] managé** (un [[opentelemetry-collector|Collector]] opéré par Azure) configuré au **niveau du Container Apps Environment**. L'agent **relaie** : il n'instrumente pas le code → l'app doit embarquer un SDK OTel ([[opentelemetry-sdk-config-otlp]]).

## Granularité : config partagée par l'Environment

**Toutes les apps du même Environment partagent la même config de destinations et de routage.** Pas d'override par app.
→ Pour router différemment deux groupes d'apps, il faut des **Environments séparés**.

- **Destinations** : App Insights (connection string), Datadog, et une ou plusieurs destinations **OTLP** nommées (fan-out possible).
- **Routage par signal** : traces / logs / metrics routés **séparément**.

## Configuration (Bicep)

```bicep
// API récente requise : la 2022-03-01 ne connaît pas openTelemetryConfiguration
openTelemetryConfiguration: {
  destinationsConfiguration: {
    otlpConfigurations: [
      {
        name: 'otel-collector'                      // nom logique, référencé plus bas
        endpoint: 'https://otel-collector.xxx:4317' // joignable depuis le VNet de l'env
        insecure: false                             // true seulement sans TLS
        headers: [ { key: 'x-api-key', value: otlpApiKey } ] // param @secure()
      }
    ]
  }
  tracesConfiguration:  { destinations: [ 'otel-collector' ] }
  logsConfiguration:    { destinations: [ 'otel-collector' ] }
  metricsConfiguration: { destinations: [ 'otel-collector' ] }
}
```

## Ce que fait Azure côté apps

Injecte automatiquement dans tous les conteneurs : `OTEL_EXPORTER_OTLP_ENDPOINT`, `OTEL_EXPORTER_OTLP_{TRACES,LOGS,METRICS}_ENDPOINT`, `OTEL_EXPORTER_OTLP_PROTOCOL`. Il reste à poser `OTEL_SERVICE_NAME` et `OTEL_RESOURCE_ATTRIBUTES` sur chaque app.

## Contraintes de routage

| Signal | App Insights | OTLP |
|---|---|---|
| Traces | ✅ | ✅ |
| Logs | ✅ | ✅ |
| Metrics | ❌ | ✅ **uniquement** |

D'où le pattern : **traces + logs → App Insights**, **metrics → OTLP** (Prometheus/Grafana).

## Limites

- Collector **bridé** : pas de pipeline custom, pas de sampling, pas de filtrage.
- Headers statiques (pas de Key Vault / managed identity).
- Pour plus de contrôle → [[container-apps-otel-collector-self-hosted|Collector auto-hébergé]], idéalement **en relais** derrière l'agent (variante hybride : les apps gardent l'injection auto, le collecteur fait le traitement).
- Attention aux **doublons** : logs écrits sur stdout (→ [[container-apps-app-logs]]) **et** via OTel.

## Voir aussi

- [[opentelemetry-collector]]
- [[container-apps-otel-collector-self-hosted]]
- [[container-apps-app-logs]] — L'autre canal : logs console/système
- [[appinsights-auto-instrumentation]]
