# Observabilité — Map of Content

Concepts généraux d'observabilité (métriques, logs, traces, APM, backends). Les **briques Azure** correspondantes sont dans [[_index-azure|l'index Azure]] (section Monitoring & Observability).

> 🗺️ Pour la vue d'ensemble côté Azure : [[azure-monitor-cartographie]].

## Fondations

- [[observabilite-piliers]] — Les 3 piliers (métriques / logs / traces), rôles et analogie
- [[apm]] — Application Performance Monitoring : la vue « boîte blanche »
- [[correlation-traces-distribuees]] — Le trio trace_id / span id / parent, propagation W3C et async
- [[cardinalite-metriques]] — Bombes à cardinalité, la règle des labels bornés
- [[vendor-lock-in]] — Niveaux de lock-in (instrumentation / requêtes / données)

## OpenTelemetry

- [[opentelemetry]] — Standard ouvert, modèle en 3 couches, niveaux d'instrumentation
- [[opentelemetry-collector]] — Pipeline receivers→processors→exporters, managé vs complet
- [[opentelemetry-sdk-config-otlp]] — Variables `OTEL_*` et mise en place du SDK par langage
- [[opentelemetry-dotnet-framework]] — Instrumenter une app ASP.NET .NET Framework 4.8 (pièges gRPC, agent App Insights)
- [[opentelemetry-collector-config]] — Config YAML type commentée (memory_limiter, filtre FinOps, auth, export)
- [[opentelemetry-collecteurs-multi-niveaux]] — Relais locaux + collecteur central, file persistante, tail sampling
- [[container-apps-otel-agent]] — Agent OTel managé Azure Container Apps *(voir [[_index-container-apps|index Container Apps]])*
- [[container-apps-otel-collector-self-hosted]] — Collecteur auto-hébergé en Container App *(idem)*

## Visualisation & backends neutres

- [[grafana]] — Dashboards, panels, multi-datasource
- [[grafana-tempo]] — Backend de traces open-source (OTLP, TraceQL)
- [[stack-lgtm]] — Loki / Grafana / Tempo / Mimir

## Mise en pratique

- [[poc-observabilite-si-hybride]] — Stratégie d'un POC hybride Azure/OnPrem en couches (hub du sujet)
- [[collecteur-otel-central-acces-reseau]] — Rendre le collecteur joignable par tout le SI : options et recommandation (env interne dédié)
- [[collecteur-otel-chemin-reseau]] — Le chemin réseau pas à pas (VNet Integration, peering, firewall, DNS, test)
