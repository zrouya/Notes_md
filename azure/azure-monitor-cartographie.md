---
tags: [azure, monitoring, observability, azure-monitor, vue-ensemble]
---

# Azure Monitor — cartographie

Vue d'ensemble de l'écosystème d'observabilité Azure. Sert à ne pas confondre *stores*, *sources* et *produits* dans le portail.

## L'idée directrice

**Azure Monitor n'est pas une ressource** qu'on déploie : c'est l'**ombrelle** qui chapeaute toute l'observabilité Azure. En dessous, tout se range en 4 familles : **données**, **sources**, **stores**, **produits**.

```
AZURE MONITOR  (plateforme, pas une ressource)
│
├─ DONNÉES : Métriques (série temporelle)  |  Logs (événements, KQL)
│
├─ SOURCES
│   ├─ Platform metrics    (auto, toute ressource)
│   ├─ Resource logs       (activés via Diagnostic settings)
│   ├─ Activity log        (plan de contrôle : qui a fait quoi)
│   ├─ Application Insights (télémétrie applicative = APM)
│   └─ Agent AMA + DCR      (OS invité des VMs)
│
├─ STORES
│   ├─ Azure Monitor Metrics    → métriques
│   ├─ Log Analytics workspace  → logs (KQL)
│   └─ Azure Monitor workspace  → métriques Prometheus managées ⚠️ nom trompeur
│
└─ PRODUITS
    ├─ Application Insights (APM : Application Map, transactions)
    ├─ Managed Prometheus / Azure Managed Grafana
    ├─ Workbooks (rapports natifs du portail)
    └─ Insights (VM / Container / Network — dashboards clés en main)
```

## Métriques vs Logs — la distinction fondatrice

| | Métriques | Logs |
|---|---|---|
| Forme | Nombres horodatés | Événements structurés/texte |
| Poids / coût | Léger, ~gratuit | Facturé à l'ingestion ([[log-analytics-cout]]) |
| Requête | Metrics Explorer / PromQL | **KQL** |
| Store | Azure Monitor Metrics | **[[log-analytics|Log Analytics]]** |

Une même donnée (une requête APIM) peut exister **des deux côtés** : métrique agrégée *et* ligne de log.

## Les confusions à dissiper

1. **Azure Monitor** = l'ombrelle ; App Insights, Log Analytics… sont *dedans*.
2. **[[log-analytics|Log Analytics]] = le magasin** ; **[[appinsights-principe|App Insights]] = un produit APM** qui *écrit dedans* (workspace-based). Pas un store parallèle.
3. **Log Analytics workspace ≠ Azure Monitor workspace** : deux stores différents (logs vs métriques Prometheus).
4. **« Diagnostic » surchargé** : les [[diagnostic-settings|Diagnostic settings]] *routent* les logs ≠ l'entité `diagnostic` interne d'une ressource qui *calibre* (sampling/verbosité).
5. **Application Insights ≠ Insights** (VM/Container Insights = dashboards préfabriqués).
6. Héritage : OMS / Log Analytics / App Insights étaient des produits **séparés** avant Azure Monitor → d'où les doublons de menus.

## Astuce portail

Pars de **la ressource** → menu **Monitoring** → *Metrics*, *Logs*, *Diagnostic settings*, *Alerts*, *Insights*. Le hub **Monitor** global agrège tout de façon transverse.

## Voir aussi

- [[log-analytics]] · [[log-analytics-cout]]
- [[appinsights-principe]] · [[appinsights-tables-requests-dependencies]] · [[apm]]
- [[diagnostic-settings]] · [[apim-gateway-logs]]
- [[grafana]] · [[_index/observabilite|Index Observabilité]]
