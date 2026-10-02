---
tags: [observability, opentelemetry, azure, container-apps, reseau, architecture]
---

# Rendre un collecteur OTel joignable depuis tout le SI

Cas d'étude : le collecteur OTel tourne en Container App, mais il doit aussi recevoir la télémétrie d'**une Web App d'une autre souscription** et de **l'on-prem**. La souscription n'est jamais l'obstacle (OTLP n'utilise pas le RBAC Azure) : **c'est le réseau**.

## Le problème : l'env Container Apps existant est « externe »

Un env externe n'a **qu'une porte publique, aucune porte côté VNet** (voir [[container-apps-environnement-externe-vs-interne]]). Donc :
- collecteur en ingress interne → joignable **uniquement** par les apps du même env ;
- la Web App (via peering) ou l'on-prem (via VPN/ExpressRoute) arrivent **par le VNet** → **aucune entrée** possible.

Et on ne peut pas passer l'env en interne sans le **recréer** ([[container-apps-choix-irreversibles]]).

## Les options

| Option | Principe | Verdict |
|---|---|---|
| **1. Nouvel env interne dédié au collecteur** | Petit env `internal: true` (workload profiles, subnet /27+) qui n'héberge que le collecteur | ✅ **Recommandé** |
| 2. Collecteur public restreint | Ingress publique + restriction d'IP + TLS + jeton | ⚠️ Temporaire seulement |
| 3. Export direct vers le backend | La Web App envoie à App Insights / SaaS sans passer par le collecteur | Simple, mais pas de traitement central |

## Pourquoi l'option 1 marche

- Les apps de l'env externe **sortent** vers les IP privées du VNet → elles atteignent le collecteur. La sortie n'a jamais été le problème, seulement l'**entrée**.
- La Web App et l'on-prem l'atteignent par l'**IP privée** de l'env interne.
- Bonus : collecteur dans un **autre env** = plus de dépendance circulaire Bicep ([[container-apps-otel-collector-self-hosted]]).
- Coût : env *workload profiles* avec le seul profil Consumption = **pas de frais fixes**, on paie les replicas.

## Pourquoi l'option 2 est mauvaise

Exposer le collecteur sur Internet oblige à **sécuriser un endpoint public** :
- **filtrer** : IP de sortie stable de la Web App ([[sortie-internet-nat-gateway|NAT Gateway]] ou firewall) + `ipSecurityRestrictions` ;
- **chiffrer** : TLS (auto avec `transport: 'http'`) ;
- **authentifier** : jeton sur le receiver, rotation à gérer.
Le NAT n'est **pas** nécessaire pour joindre l'IP publique : il sert à avoir une **IP source fixe** pour pouvoir filtrer.
Pour l'on-prem, c'est pire : des serveurs qui sortent sur Internet, souvent interdit.

## Décisions à prendre dès la création de l'env dédié

Interne · workload profiles · redondance de zone · **spoke partagé** (service transverse, géré par l'équipe plateforme) · subnet /26 ou /25 pour un service central.

## Voir aussi

- [[collecteur-otel-chemin-reseau]] — le chemin réseau pas à pas
- [[opentelemetry-collecteurs-multi-niveaux]]
- [[opentelemetry-dotnet-framework]]
- [[poc-observabilite-si-hybride]]
