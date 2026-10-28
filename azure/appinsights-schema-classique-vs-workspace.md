---
tags: [azure, appinsights, log-analytics, kql, schema]
---

# App Insights : schéma « classique » vs schéma « workspace »

La **même télémétrie** [[appinsights-principe|Application Insights]] est exposée sous **deux schémas KQL**. Tout dépend de la portée de la requête : la ressource App Insights ou le [[log-analytics|workspace Log Analytics]].

## D'où ça vient

- **Ressource classique** (retirée en 2024) : App Insights stockait ses données lui-même, dans le schéma `requests`, `dependencies`…
- **Ressource workspace-based** (la seule aujourd'hui) : les données sont **physiquement stockées dans le workspace**, dans les tables `AppRequests`, `AppDependencies`…
- Pour la compatibilité, interroger **la ressource App Insights** présente toujours l'ancien schéma, comme une vue sur les tables du workspace.

> « Classique » désigne ici **le schéma**, pas une ressource classique.

## Correspondance des tables

| Portée App Insights (classique) | Portée workspace |
|---|---|
| `requests` | `AppRequests` |
| `dependencies` | `AppDependencies` |
| `traces` | `AppTraces` |
| `exceptions` | `AppExceptions` |
| `customEvents` / `pageViews` | `AppEvents` / `AppPageViews` |
| `customMetrics` / `performanceCounters` | `AppMetrics` / `AppPerformanceCounters` |
| `availabilityResults` / `browserTimings` | `AppAvailabilityResults` / `AppBrowserTimings` |

## Correspondance des colonnes

| Classique | Workspace |
|---|---|
| `timestamp` | `TimeGenerated` |
| `operation_Id` / `operation_ParentId` / `id` | `OperationId` / `ParentId` / `Id` |
| `duration` | `DurationMs` |
| `cloud_RoleName` | `AppRoleName` |
| `customDimensions` / `customMeasurements` | `Properties` / `Measurements` |
| `itemCount` | `ItemCount` |

## Quand utiliser l'un ou l'autre

- **Portée App Insights** : une seule application. C'est le schéma attendu par les outils historiques, dont le type de requête *Traces* de [[grafana-waterfall-azure-monitor|Grafana]].
- **Portée workspace** : plusieurs composants App Insights peuvent écrire dans le même workspace. Filtrer alors par **`_ResourceId`**. Cette portée permet aussi de joindre d'autres tables, comme les [[apim-gateway-logs|GatewayLogs]].

```kql
// Requêtes croisées depuis une seule portée
app("mon-appinsights").requests | take 10
workspace("mon-workspace").AppRequests | take 10
```

## Voir aussi

- [[appinsights-modele-donnees-otel]]
- [[appinsights-tables-requests-dependencies]]
- [[log-analytics]]
