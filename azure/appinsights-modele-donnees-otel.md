---
tags: [azure, appinsights, opentelemetry, tracing, spans]
---

# Modèle de données App Insights ↔ OpenTelemetry

Correspondance entre les concepts [[opentelemetry|OpenTelemetry]] (trace, span, log) et les tables d'[[appinsights-principe|Application Insights]].

> ⚠️ **La table `traces` / `AppTraces` ne contient pas de traces au sens OTel : ce sont des logs.** Le nom date d'avant OTel, quand « trace » désignait une ligne de log de diagnostic.

## Rappel OTel

- **Trace** : un traitement de bout en bout, identifié par un `TraceId`. Une trace est un **ensemble de spans**.
- **Span** : une unité de travail, avec un `SpanId`, un `ParentSpanId`, un début, une durée et un *kind*. Le **waterfall est l'arbre des spans**.
- **Log** : un enregistrement ponctuel, sans durée. Il peut porter `TraceId` et `SpanId` pour être corrélé, mais **ce n'est pas un span**.

Chez App Insights, une trace s'appelle une **opération** (voir [[correlation-traces-distribuees]]).

## Les tables (schémas : voir [[appinsights-schema-classique-vs-workspace]])

| Table | Contenu | Span ? |
|---|---|---|
| `requests` / `AppRequests` | Spans **entrants** : kind *Server*, *Consumer* | **Oui** |
| `dependencies` / `AppDependencies` | Spans **sortants ou internes** : kind *Client*, *Producer*, *Internal* (type `InProc`) | **Oui** |
| `traces` / `AppTraces` | **Logs** (`ILogger`, `console.log`…) | Non |
| `exceptions` / `AppExceptions` | Exceptions rattachées à un span | Non (événement de span) |
| `customEvents`, `pageViews`… | Événements métier, navigation navigateur | Non, mais corrélés |

## Un OperationId présent seulement dans AppTraces

Un `OperationId` trouvé dans les logs prouve qu'un `TraceId` existait. Il ne prouve pas que des **spans ont été collectés**. Causes possibles :
- code exécuté sans span exporté (worker, timer, source d'`Activity` non écoutée) ;
- spans **échantillonnés** alors que les logs ont été conservés ;
- spans envoyés vers **une autre ressource** App Insights.

```kql
let op = "<OperationId>";
union AppRequests, AppDependencies, AppTraces, AppExceptions
| where OperationId == op
| summarize nb = count() by Type, _ResourceId
```

## Voir aussi

- [[appinsights-tables-requests-dependencies]]
- [[grafana-waterfall-azure-monitor]]
- [[correlation-traces-distribuees]]
- [[container-apps-console-logs-vs-apptraces]] — `AppTraces` vs logs console des conteneurs
