
**APM = Application Performance Monitoring** : catégorie d'outils qui surveillent les applications **de l'intérieur** (côté code, pas côté infra).

## Questions auxquelles un APM répond

- Quelles opérations sont les plus lentes, et *pourquoi* ?
- Où le temps est passé *dans* une transaction (méthode, requête SQL, appel externe) ?
- Quel est le taux d'erreurs, et quelles exceptions les causent ?
- Comment les services s'appellent entre eux (carte de dépendances) ?

## Comment

S'appuie surtout sur les **traces** (voir [[Corrélation de traces distribuées]]) et les métriques applicatives, présentées dans une UI de diagnostic : chronologie de transaction, waterfall de spans, service map.

## Exemples

- **[[Application Insights]]** (l'APM d'Azure)
- Datadog APM, New Relic, Dynatrace
- **[[Grafana Tempo]]** (open-source, côté traces)

## Lien

C'est la vue **« boîte blanche »** de l'observabilité — obtenue via l'instrumentation [[OpenTelemetry]] des applications.

## Voir aussi

- [[Observabilité - métriques, logs et traces]]
- [[Application Insights]]
- [[OpenTelemetry]]
