
**Grafana** est un outil de **dashboards** d'observabilité, faits de **panels**. Chaque panel = *une requête sur une datasource* + *une visualisation*.

## Multi-datasource

Sa force : il lit de nombreuses sources — **Azure Monitor / [[Log Analytics]]** (KQL), Prometheus (PromQL), Loki, **[[Grafana Tempo|Tempo]]**… et peut les unifier dans un même dashboard.

## Types de panels ↔ piliers

| Donnée | Panel |
|---|---|
| Métriques / RED | *Time series*, *Stat*, *Gauge*, *Table*, *Bar* |
| Logs | *Logs* (flux/table) |
| Traces | *Traces* (waterfall de spans) |

## Le waterfall de traces

Le panel *Traces* affiche le waterfall. Il s'alimente soit depuis **[[Application Insights]]** (requête *Traces* de la datasource Azure Monitor, lit `AppRequests`/`AppDependencies`), soit depuis **[[Grafana Tempo|Tempo]]**.

## Accès à Azure

Via **Managed Identity** (aucun secret) quand Grafana tourne dans Azure, avec le rôle `Monitoring Reader`.

## Voir aussi

- [[Grafana Tempo]]
- [[Stack LGTM]]
- [[Log Analytics]]
