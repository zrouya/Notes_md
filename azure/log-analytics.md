---
tags: [azure, monitoring, observability, log-analytics, kql]
---

# Log Analytics

**Log Analytics (workspace)** est le **magasin** de logs d'Azure Monitor : une base de **tables** interrogée en **KQL** (langage Kusto). La fondation où tout finit stocké.

## Ce qui atterrit dedans

- Les **resource logs** routés par les [[diagnostic-settings|diagnostic settings]] (ex. [[apim-gateway-logs|ApiManagementGatewayLogs]]).
- Les tables d'**[[appinsights-principe|Application Insights]]** ([[appinsights-tables-requests-dependencies|AppRequests, AppDependencies]]…), puisqu'App Insights workspace-based écrit ici.
- Les métriques envoyées explicitement (table `AzureMetrics`).

## À ne pas confondre

- **Log Analytics = le magasin** (stockage + moteur KQL).
- **App Insights = un produit APM** qui *alimente* ce magasin.

## Accès

[[grafana|Grafana]] lit le workspace via la datasource Azure Monitor (KQL), en plus des métriques natives.

## Voir aussi

- [[appinsights-principe]]
- [[diagnostic-settings]]
- [[log-analytics-cout]]
