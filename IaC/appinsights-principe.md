---
tags: [azure, monitoring, appinsights, observability]
---

# Application Insights — Principe général

Service APM (Application Performance Monitoring) d'Azure Monitor. Collecte des télémétries : requêtes, logs, métriques, traces, exceptions, dépendances.

## Lien app ↔ AppInsights

La connexion se fait via une **Connection String** injectée comme variable d'environnement :

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=<guid>;IngestionEndpoint=https://...
```

Le SDK ou l'agent lit cette variable automatiquement au démarrage.

## Flux de données

```
App (SDK ou agent)
  → buffer mémoire (~30s ou ~500 items)
  → HTTPS POST vers IngestionEndpoint
  → Azure Monitor
      ├── Logs → Log Analytics Workspace (requêtes KQL)
      ├── Métriques → Azure Metrics
      └── Live Metrics → stream temps réel
```

## Deux modes de collecte

| Mode | Principe |
|---|---|
| **SDK** | Package ajouté au code, API custom disponible |
| **Codeless (agent)** | Agent injecté par Azure, aucun code modifié |

## Détails

- Chaque runtime a son propre SDK/agent (.NET, Java, Node.js, Python)
- La connection string est préférée à l'instrumentation key (dépréciée)
- Sans SDK ni agent : les diagnostic settings peuvent forwarder stdout/stderr vers Log Analytics, mais sans traces distribuées ni dépendances

## Voir aussi

- [[appinsights-auto-instrumentation]]
- [[appinsights-agent-injection]]
- [[azure-containerapp-sql-access]]
