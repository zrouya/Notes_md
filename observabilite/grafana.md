---
tags: [observability, grafana, dashboards, visualization]
---

# Grafana

Outil de **dashboards** d'observabilité, faits de **panels**. Chaque panel = *une requête sur une datasource* + *une visualisation*.

## Multi-datasource

Sa force : il lit de nombreuses sources — **Azure Monitor / [[log-analytics|Log Analytics]]** (KQL), Prometheus (PromQL), Loki, **[[grafana-tempo|Tempo]]** — et peut les unifier.

## Types de panels ↔ piliers

| Donnée | Panel |
|---|---|
| Métriques / RED | *Time series*, *Stat*, *Gauge*, *Table*, *Bar* |
| Logs | *Logs* (flux/table) |
| Traces | *Traces* (waterfall de spans) |

## Le waterfall de traces

Le panel *Traces* s'alimente soit depuis **[[appinsights-principe|App Insights]]** (requête *Traces* de la datasource Azure Monitor, lit [[appinsights-tables-requests-dependencies|AppRequests/AppDependencies]]), soit depuis **[[grafana-tempo|Tempo]]**.

## Accès à Azure

Via **Managed Identity** (aucun secret) quand Grafana tourne dans Azure, avec le rôle `Monitoring Reader`.

## Voir aussi

- [[grafana-tempo]]
- [[stack-lgtm]]
- [[log-analytics]]
