
[[Azure Container Apps]] fournit un **agent [[OpenTelemetry]] managé** (un [[OpenTelemetry Collector|Collector]] opéré par Azure) configuré au **niveau du Container Apps Environment**.

## Granularité : config partagée par l'Environment

**Toutes les apps du même Environment partagent la même config de destinations et de routage.** Il n'y a **pas** d'override par app.
→ Pour router différemment deux groupes d'apps, il faut des **Environments séparés**.

Ce qu'on peut régler :
- **Destinations** : une App Insights (connection string) + une ou plusieurs destinations **OTLP** nommées (fan-out possible).
- **Routage par signal** : traces / logs / metrics routés **séparément** vers ces destinations (mais toujours au niveau Environment).

Azure **injecte automatiquement** `OTEL_EXPORTER_OTLP_ENDPOINT` dans tous les conteneurs → l'app émet juste de l'OTLP, sans endpoint en dur.

## Contraintes de routage

| Signal | App Insights | OTLP |
|---|---|---|
| Traces | ✅ | ✅ |
| Logs | ✅ | ✅ |
| Metrics | ❌ | ✅ **uniquement** |

D'où le pattern courant : **traces + logs → App Insights**, **metrics → OTLP** ([[Prometheus]]/Grafana).

## Structure ARM (source de vérité)

```jsonc
"properties": {
  "appInsightsConfiguration": { "connectionString": "..." },
  "openTelemetryConfiguration": {
    "destinationsConfiguration": {
      "otlpConfigurations": [
        { "name": "grafana-otlp", "endpoint": "otel-collector:4317", "insecure": false }
      ]
    },
    "tracesConfiguration":  { "destinations": ["appInsights", "grafana-otlp"] },
    "logsConfiguration":    { "destinations": ["appInsights"] },
    "metricsConfiguration": { "destinations": ["grafana-otlp"] }
  }
}
```

## Terraform

Support natif `azurerm` arrivé tardivement → **vérifier la version du provider**. Repli fiable : **`azapi`** (`azapi_update_resource` sur `Microsoft.App/managedEnvironments`) pour piloter directement `properties.openTelemetryConfiguration`.

## Limite

Collector volontairement bridé : pas de pipeline custom, pas de tail sampling, pas de transformation avancée. Pour ça → [[OpenTelemetry Collector|Collector auto-hébergé]] en relais.

## Voir aussi

- [[OpenTelemetry]]
- [[OpenTelemetry Collector]]
- [[Azure Container Apps]]
