
**Application Insights** est l'**[[APM (Application Performance Monitoring)|APM]] d'Azure** : il collecte la télémétrie *applicative* (requêtes, dépendances, exceptions, traces) et fournit une UI de diagnostic.

## Point clé : ce n'est pas un magasin séparé

Dans le modèle **workspace-based** (le défaut moderne), App Insights **écrit ses données DANS un [[Log Analytics]]**. C'est un **produit posé au-dessus** du magasin, pas une base concurrente.

## Ce qu'il capture (tables)

- `AppRequests` — les appels reçus
- `AppDependencies` — les appels émis (HTTP, SQL, Service Bus…)
- `AppExceptions`, `AppTraces`

Voir [[Tables AppRequests et AppDependencies]].

## UI

Application Map, Live Metrics, **End-to-end transaction details** (reconstruit le waterfall automatiquement).

## Ingestion

Via SDK App Insights **ou** [[OpenTelemetry]] (Azure Monitor OTel Distro). Prend en charge la **corrélation W3C** ([[Corrélation de traces distribuées]]).

## Voir aussi

- [[Log Analytics]]
- [[APM (Application Performance Monitoring)]]
- [[Tables AppRequests et AppDependencies]]
