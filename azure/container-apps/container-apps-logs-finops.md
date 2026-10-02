---
tags: [azure, container-apps, logs, finops, log-analytics]
---

# Logs Container Apps : `log-analytics` vs `azure-monitor` (FinOps)

Passer `appLogsConfiguration.destination` de `log-analytics` à `azure-monitor` **ne change pas le prix au Go** : ça **débloque les leviers d'économie**. Rappel du modèle : [[container-apps-app-logs]].

## Ce qui ne change pas

- **Prix d'ingestion** dans le workspace : identique. Pas de frais d'export pour des [[diagnostic-settings]] **vers Log Analytics** (frais seulement vers Storage / Event Hub / partenaire).
- **Volume** : à peu près le même à périmètre égal.
- Le réglage reste **au niveau de l'Environment** (commun à toutes les apps).

## Ce que `azure-monitor` permet en plus

| Levier | Explication |
|---|---|
| **Choix des catégories** | Garder `ContainerAppConsoleLogs`, couper `ContainerAppSystemLogs` (ou l'inverse). En `log-analytics` : tout ou rien. |
| **Plan de table Basic / Auxiliary** | Ingestion bien moins chère sur `ContainerAppConsoleLogs`. Contreparties : requêtes facturées, KQL limité, alertes restreintes, rétention interactive courte. Les tables `_CL` legacy n'y ont pas droit. |
| **Transformation à l'ingestion** (DCR workspace) | Supprimer lignes (DEBUG, health checks, une app précise) ou colonnes **avant facturation** → permet un filtrage **par app** malgré la config commune. |
| **Multi-destinations** | LAW à rétention courte + Storage Account (archivage pas cher, export facturé au Go). |

Le plan Basic est souvent **le levier le plus fort** pour des apps bavardes. Coûts généraux : [[log-analytics-cout]].

## Points de vigilance lors de la bascule

- **Double ingestion** si on garde les deux circuits en parallèle.
- **Changement de tables** : `ContainerAppConsoleLogs_CL` → `ContainerAppConsoleLogs`, colonnes `Log_s`, `ContainerAppName_s`… renommées → **réécrire** requêtes KQL, alertes, workbooks.
- **Rétention par table** à reconfigurer.
- **Commitment tier** : baisser le volume ne rapporte que si on change de palier.

## En Bicep

```bicep
appLogsConfiguration: { destination: 'azure-monitor' }   // plus de logAnalyticsConfiguration
// + une ressource Microsoft.Insights/diagnosticSettings avec scope: containerAppEnv
```

## Voir aussi

- [[container-apps-app-logs]]
- [[diagnostic-settings]]
- [[log-analytics-cout]]
