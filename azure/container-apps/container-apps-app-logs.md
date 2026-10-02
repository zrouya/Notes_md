---
tags: [azure, container-apps, monitoring, logs, log-analytics]
---

# Container Apps — modèle de logs (appLogsConfiguration)

Les **Container Apps ne suivent pas** le modèle standard des *resource logs* / [[diagnostic-settings|diagnostic settings]]. Leur journalisation se configure **au niveau de l'Environment**, via la propriété **`appLogsConfiguration`**.

→ Conséquence : **pas de blade « Diagnostic settings »** sur la Container App ni sur l'Environment (en mode direct), et elles **n'apparaissent pas** dans le hub central Monitor → Diagnostic settings.

## Les 3 modes de destination

| `destination` | Où vont les logs | Diagnostic settings visible ? |
|---|---|---|
| **`log-analytics`** | Directement au workspace (`customerId` + `sharedKey`) | ❌ Non — court-circuite le pipeline Azure Monitor |
| **`azure-monitor`** | Via le pipeline Azure Monitor → routable multi-destinations | ✅ Oui (blade sur l'Environment) |
| **`none`** | Nulle part | ❌ |

```bicep
appLogsConfiguration: {
  destination: 'log-analytics'
  logAnalyticsConfiguration: {
    customerId: '<workspace customerId>'
    sharedKey:  '<workspace key>'
  }
}
```

## Où sont les logs (mode log-analytics)

- **`ContainerAppConsoleLogs_CL`** — logs applicatifs (stdout/stderr des conteneurs)
- **`ContainerAppSystemLogs_CL`** — événements système (scaling, redémarrages…)

```kql
ContainerAppConsoleLogs_CL
| where TimeGenerated > ago(1h)
| project TimeGenerated, ContainerAppName_s, Log_s
| order by TimeGenerated desc
```

⚠️ Le suffixe **`_CL`** (*custom log*) est propre au mode `log-analytics` direct. En mode `azure-monitor`, les tables changent de nom (`ContainerAppConsoleLogs` sans `_CL`).

## Les 3 canaux de télémétrie d'une Container App

À ne pas confondre — ce sont des mécanismes **distincts** :

1. **`appLogsConfiguration`** — logs console / système (cette note).
2. **[[container-apps-otel-agent|Agent OpenTelemetry managé]]** — OTLP (traces / logs / métriques vers App Insights ou un endpoint OTLP).
3. **Métriques natives** — dans le blade *Metrics*, toujours dispo (requêtes/s, CPU, mémoire, réplicas), indépendamment du reste.

Les logs console ≠ l'équivalent des `GatewayLogs` de l'APIM : ce sont des logs bruts, pas de l'observabilité applicative structurée. Pour celle-ci → instrumentation [[opentelemetry|OTel]].

## Voir aussi

- [[container-apps-otel-agent]]
- [[diagnostic-settings]]
- [[log-analytics]]
- [[azure-monitor-cartographie]]
