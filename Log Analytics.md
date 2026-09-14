
**Log Analytics (workspace)** est le **magasin** de logs d'Azure Monitor : une base de **tables** interrogée en **KQL** (langage Kusto). C'est la fondation où tout finit stocké.

## Ce qui atterrit dedans

- Les **resource logs** routés par les [[Diagnostic settings Azure Monitor|diagnostic settings]] (ex. `ApiManagementGatewayLogs`).
- Les tables d'**[[Application Insights]]** (`AppRequests`, `AppDependencies`…), puisqu'App Insights workspace-based écrit ici.
- Les métriques envoyées explicitement (table `AzureMetrics`).

## À ne pas confondre

- **Log Analytics = le magasin** (le stockage + le moteur KQL).
- **App Insights = un produit APM** qui *alimente* ce magasin.

## Accès

[[Grafana]] lit le workspace via la datasource Azure Monitor (requêtes KQL), en plus des métriques natives.

## Voir aussi

- [[Application Insights]]
- [[Diagnostic settings Azure Monitor]]
- [[Coût de Log Analytics]]
