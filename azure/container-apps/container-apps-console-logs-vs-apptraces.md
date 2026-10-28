---
tags: [azure, container-apps, appinsights, logs, finops]
---

# ContainerAppConsoleLogs_CL vs AppTraces

Deux tables qui contiennent toutes deux des logs applicatifs. Elles diffèrent par **qui collecte** et par **ce qu'elles portent**.

> **Console logs = ce que le conteneur écrit sur stdout/stderr (capté par la plateforme). `AppTraces` = ce que l'app envoie via son logger instrumenté (corrélé aux traces).**

## Comparaison

| | `ContainerAppConsoleLogs_CL` | `AppTraces` |
|---|---|---|
| Collecteur | La **plateforme** Container Apps ([[container-apps-app-logs\|appLogsConfiguration]]) | L'**app** : SDK App Insights, distro Azure Monitor OTel, [[container-apps-otel-agent\|agent OTel]] |
| Instrumentation | Aucune | Requise |
| Format | Texte brut `Log_s` + `ContainerAppName_s`, `RevisionName_s`, `ContainerName_s`, `Stream_s` | Structuré : `Message`, `SeverityLevel`, `Properties` |
| Corrélation | **Aucune**, sauf si l'ID est écrit dans le texte | **`OperationId` / `ParentId`** → visible dans la waterfall |
| Exclusivités | Crash au démarrage, sortie de process tiers, erreurs avant l'initialisation du SDK | Logs structurés, rattachés au span actif |
| Filtrage | Tout ce qui sort sur la console | Niveau du logger, sampling possible (`ItemCount`) |

Sans `_CL` (`ContainerAppConsoleLogs`) en mode `azure-monitor`. Voir [[container-apps-logs-finops]].
`AppTraces` contient **des logs, pas des spans** : voir [[appinsights-modele-donnees-otel]].

## Piège : double ingestion

Une app .NET qui garde le **console logger** actif et exporte aussi vers App Insights écrit **chaque log deux fois** et paie deux fois l'ingestion ([[log-analytics-cout]]).

Répartition conseillée :
- **`AppTraces`** pour le diagnostic applicatif, car il est corrélé ;
- **console logs** comme filet de sécurité (démarrage, crash). En prod, console logger à `Warning`, ou désactivé.

```json
// appsettings.Production.json
"Logging": { "Console": { "LogLevel": { "Default": "Warning" } } }
```

## Piège : logs « manquants » dans AppTraces

Le provider `ILogger` du **SDK App Insights classique** ne capte par défaut que **`Warning` et au-dessus**. Des logs `Information` visibles dans la console mais absents de `AppTraces` ne sont pas un bug : il faut configurer `Logging:ApplicationInsights:LogLevel`.

## Voir aussi

- [[container-apps-app-logs]]
- [[container-apps-logs-finops]]
- [[appinsights-modele-donnees-otel]]
