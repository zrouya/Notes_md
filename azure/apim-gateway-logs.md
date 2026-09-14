---
tags: [azure, apim, monitoring, gateway-logs, kql]
---

# Table ApiManagementGatewayLogs

Table [[log-analytics|Log Analytics]] alimentée par les [[diagnostic-settings|diagnostic settings]] de l'APIM (catégorie `GatewayLogs`). **Une ligne par requête** passant par le gateway — la vue **gateway-centrée**.

## Colonnes clés

- **Identité** : `ApiId`, `OperationId`, `ProductId`, `ApimSubscriptionId`
- **HTTP** : `Method`, `Url`, `ResponseCode`, `BackendResponseCode`, `IsRequestSuccess`
- **⏱ Latence** : `TotalTime`, `BackendTime` (ms)
- **Backend** : `BackendId`, `BackendUrl`
- **Erreurs** : `LastErrorReason`, `LastErrorSource`, `LastErrorMessage`
- **Divers** : `Cache`, `RequestSize`, `ResponseSize`, `Region`, `CorrelationId`

## Sa superpuissance

Le couple **`TotalTime` / `BackendTime`** : `TotalTime - BackendTime` = temps passé *dans le gateway*. Seule vue donnant d'emblée le **découpage gateway vs backend** → idéal pour les dashboards RED.

## Exemple KQL

```kql
ApiManagementGatewayLogs
| summarize requests=count(),
            errors=countif(ResponseCode >= 500),
            p95_total=percentile(TotalTime,95),
            p95_backend=percentile(BackendTime,95)
  by ApiId, BackendId
```

Pas de sampling (comptage direct), contrairement aux tables App Insights.

## Voir aussi

- [[appinsights-tables-requests-dependencies]]
- [[diagnostic-settings]]
