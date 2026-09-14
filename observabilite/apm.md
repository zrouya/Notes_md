---
tags: [observability, apm, tracing, concepts]
---

# APM (Application Performance Monitoring)

Catégorie d'outils qui surveillent les applications **de l'intérieur** (côté code, pas côté infra).

## Questions auxquelles un APM répond

- Quelles opérations sont les plus lentes, et *pourquoi* ?
- Où le temps est passé *dans* une transaction (méthode, requête SQL, appel externe) ?
- Quel est le taux d'erreurs, et quelles exceptions les causent ?
- Comment les services s'appellent entre eux (carte de dépendances) ?

## Comment

S'appuie surtout sur les **traces** ([[correlation-traces-distribuees]]) et les métriques applicatives, dans une UI de diagnostic : chronologie de transaction, waterfall de spans, service map.

## Exemples

- **[[appinsights-principe|Application Insights]]** (l'APM d'Azure)
- Datadog APM, New Relic, Dynatrace
- **[[grafana-tempo|Grafana Tempo]]** (open-source, côté traces)

## Lien

C'est la vue **« boîte blanche »** de l'observabilité — obtenue via l'instrumentation [[opentelemetry|OpenTelemetry]] des applications.

## Voir aussi

- [[observabilite-piliers]]
- [[appinsights-principe]]
- [[opentelemetry]]
