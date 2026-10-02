---
tags: [observability, opentelemetry, collector, architecture, hybride]
---

# Collecteurs OpenTelemetry à plusieurs niveaux (agent / gateway)

Quand on centralise la télémétrie de tout un SI, on ne fait pas envoyer chaque application directement au collecteur central : on enchaîne **des collecteurs relais locaux** et **un collecteur central (gateway)**.

## Schéma

```
Apps on-prem ──► collecteur relais (par site/DC) ──WAN──┐
Apps Azure   ──► (agent managé ou direct) ──────────────┼──► collecteur central ──► backend(s)
Web Apps     ──► ───────────────────────────────────────┘
```

## Rôle du collecteur relais (on-prem)

- Les apps envoient **en local** : latence faible, pas de dépendance au WAN.
- **Batch + compression gzip** : économise la bande passante.
- **File d'attente persistante** sur disque (extension `file_storage` sur l'exporteur) : **rien n'est perdu** pendant une coupure VPN / ExpressRoute.
- **Un seul flux** à ouvrir sur les firewalls (relais → central) au lieu d'un par serveur.
- Premier **filtrage** possible avant le WAN.

## Rôle du collecteur central

- Traitement commun : filtrage, enrichissement, échantillonnage, routage vers les backends.
- **Service critique** pour tout le SI → plusieurs replicas, autoscaling, redondance de zone.

## Le cas du tail sampling

Le *tail sampling* décide de garder une trace **après l'avoir vue en entier** (ex. garder toutes les traces en erreur). Il faut donc que **tous les spans d'une trace arrivent sur la même instance**.
- Avec plusieurs replicas : une 1re couche avec `loadbalancingexporter` (qui répartit **par trace_id**), puis la couche qui échantillonne.
- Sans ce besoin : `filter` + `probabilistic_sampler` côté SDK suffisent.

## Sécurité multi-sources

- Un jeton par source / équipe, ou **mTLS** avec la PKI interne.
- Imposer des attributs de ressource (`service.name`, `service.namespace`, `deployment.environment`, site) pour savoir d'où viennent les données.

## Voir aussi

- [[opentelemetry-collector]]
- [[opentelemetry-collector-config]]
- [[collecteur-otel-central-acces-reseau]]
- [[poc-observabilite-si-hybride]]
