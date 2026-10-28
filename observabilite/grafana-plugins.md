---
tags: [observability, grafana, plugins]
---

# Plugins Grafana

Extensions qui ajoutent à [[grafana]] ce que le cœur ne fait pas : une source de données, une visualisation ou une application complète.

## Les 3 types

| Type | Rôle | Exemples |
|---|---|---|
| **Data source** | Interroger un stockage / une API non supporté nativement | Azure Monitor, Elasticsearch, MongoDB, API REST maison |
| **Panel** | Nouveau type de visualisation dans un dashboard | Gantt, flowchart, carte, heatmap avancée |
| **App** | Bundle : pages dédiées + datasources + panels + dashboards préconfigurés | Kubernetes Monitoring, Zabbix, OnCall |

## À quoi ça sert

- **Visualiser la donnée là où elle est**, sans la recopier dans Prometheus ou une base intermédiaire
- **Unifier** : un dashboard croise métriques Azure, logs Elasticsearch, données métier
- **Alerter** sur ces sources (nécessite un backend, voir [[grafana-plugin-structure]])
- **Couvrir un besoin spécifique** quand rien n'existe au catalogue

## Avant d'en développer un

Vérifier le catalogue, puis les plugins génériques : pour une API JSON/CSV/XML, [[grafana-infinity]] suffit souvent.

## Voir aussi

- [[grafana-plugin-structure]]
- [[grafana-plugin-installation]]
- [[grafana-infinity]]
- [[grafana]]
