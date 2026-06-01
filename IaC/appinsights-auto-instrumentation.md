---
tags: [azure, appinsights, container-apps, codeless, monitoring]
---

# AppInsights — Auto-instrumentation codeless sur Container Apps

Azure Container Apps peut instrumenter une app .NET ou Java **sans modifier le code** ni ajouter de package.

## Mécanisme

Azure utilise un **init container** pour déposer l'agent dans un volume partagé, puis injecte les variables d'environnement dans le container applicatif :

```
[init container]  →  copie agent → /agents/
[container app]   →  CORECLR_ENABLE_PROFILING=1
                      CORECLR_PROFILER_PATH=/agents/...
                      APPLICATIONINSIGHTS_CONNECTION_STRING=...
```

Le runtime (.NET CLR ou JVM) charge l'agent automatiquement au démarrage.

## Activation (Bicep)

```bicep
env: [
  {
    name: 'APPLICATIONINSIGHTS_CONNECTION_STRING'
    value: appInsightsConnectionString
  }
]
```

Azure détecte le runtime et injecte l'agent approprié.

## SDK vs Codeless

| | Codeless | SDK |
|---|---|---|
| Requêtes HTTP | ✅ | ✅ |
| Dépendances (SQL, HTTP out) | ✅ | ✅ |
| Exceptions | ✅ | ✅ |
| Logs custom (ILogger) | Partiel | ✅ |
| Events/métriques custom | ❌ | ✅ |
| Traces distribuées custom | Limité | ✅ |

## Runtimes supportés en codeless (Container Apps)

- **.NET** ✅
- **Java** ✅
- Node.js, Python → SDK requis

## Voir aussi

- [[appinsights-principe]]
- [[clr-profiler]]
- [[java-agent]]
