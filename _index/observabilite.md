# Observabilité — Map of Content

Concepts généraux d'observabilité (métriques, logs, traces, APM, backends). Les **briques Azure** correspondantes sont dans [[_index/azure|l'index Azure]] (section Monitoring & Observability).

## Fondations

- [[observabilite-piliers]] — Les 3 piliers (métriques / logs / traces), rôles et analogie
- [[apm]] — Application Performance Monitoring : la vue « boîte blanche »
- [[correlation-traces-distribuees]] — Le trio trace_id / span id / parent, propagation W3C et async
- [[cardinalite-metriques]] — Bombes à cardinalité, la règle des labels bornés
- [[vendor-lock-in]] — Niveaux de lock-in (instrumentation / requêtes / données)

## OpenTelemetry

- [[opentelemetry]] — Standard ouvert, modèle en 3 couches, niveaux d'instrumentation
- [[opentelemetry-collector]] — Pipeline receivers→processors→exporters, managé vs complet
- [[container-apps-otel-agent]] — Agent OTel managé Azure Container Apps *(voir index Azure)*

## Visualisation & backends neutres

- [[grafana]] — Dashboards, panels, multi-datasource
- [[grafana-tempo]] — Backend de traces open-source (OTLP, TraceQL)
- [[stack-lgtm]] — Loki / Grafana / Tempo / Mimir

## Mise en pratique

- [[poc-observabilite-si-hybride]] — Stratégie d'un POC hybride Azure/OnPrem en couches (hub du sujet)
