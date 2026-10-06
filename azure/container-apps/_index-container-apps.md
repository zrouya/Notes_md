---
tags: [index, azure, container-apps]
---

# Azure — Container Apps

Notes sur Azure Container Apps : réseau et types d'Environment, plan Consumption, logs et instrumentation OpenTelemetry. Première lecture conseillée **dans l'ordre**.

## Parcours de lecture

| # | Note | Ce qu'on y apprend |
|---|------|--------------------|
| 1 | [[container-apps-environnement-externe-vs-interne]] | Le mode de l'env = où est sa porte d'entrée ; pourquoi « externe » complique l'accès privé |
| 2 | [[container-apps-ingress-dns-interne]] | Flag `external` d'une app, HTTP vs TCP, noms DNS automatiques, domaine perso |
| 3 | [[container-apps-choix-irreversibles]] | Ce qui est figé à la création (mode, type d'env, zones) |
| 4 | [[container-apps-workload-profiles]] | *Consumption only* vs *Workload profiles* : réseau, profils, détection, migration |
| 5 | [[container-apps-plan-consumption]] | Le serverless : facturation à la seconde, scale to zero, tarif idle, limites |
| 5b | [[container-apps-consumption-only-architecture]] | AKS sous-jacent, IP pré-réservées par nœud, diagnostic du subnet |
| 6 | [[container-apps-app-logs]] | Modèle de logs (`appLogsConfiguration`), tables `_CL` |
| 7 | [[container-apps-logs-finops]] | `log-analytics` vs `azure-monitor` : impacts coût et leviers |
| 8 | [[container-apps-otel-agent]] | Agent OTel managé : config Bicep, variables injectées, limites |
| 9 | [[container-apps-otel-collector-self-hosted]] | Héberger son propre collecteur, piège de la dépendance circulaire |

## Cas pratique

- [[collecteur-otel-central-acces-reseau]] — collecteur joignable depuis une Web App d'une autre souscription et l'on-prem
- [[collecteur-otel-chemin-reseau]] — le chemin réseau pas à pas

## Voir aussi

- [[_index-observabilite|Index Observabilité]] (OpenTelemetry, collecteur, SDK)
- [[_index-reseau-vnet|Index Réseau Azure]]
- [[_index-azure|Index Azure]]
