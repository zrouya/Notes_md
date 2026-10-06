---
tags: [azure, container-apps, architecture, checklist]
---

# Container Apps Environment : choix irréversibles à la création

Certains paramètres d'un Environment sont **figés à la création** : l'infrastructure sous-jacente (load balancer, IP, zones) est provisionnée à ce moment-là. Les changer = **recréer l'env et redéployer toutes ses apps**.

## Checklist avant de créer un env

| Décision | Options | Recommandation landing zone |
|---|---|---|
| **Mode** (`internal`) | Externe / Interne | **Interne** → [[container-apps-environnement-externe-vs-interne]] |
| **Type d'env** | *Consumption only* (legacy) / *Workload profiles* | **Workload profiles** → [[container-apps-workload-profiles]] |
| **Redondance de zone** (`zoneRedundant`) | Oui / Non | **Oui** pour un service critique (si la région le permet) |
| **Subnet** | Taille, VNet d'accueil | Penser au scaling futur |

## Consumption only vs Workload profiles

| | Consumption only (legacy) | Workload profiles |
|---|---|---|
| Taille mini du subnet | **/23** | **/27** |
| Profils dédiés (CPU/RAM réservés) | ❌ | ✅ (optionnel) |
| UDR, NAT Gateway sur le subnet | ❌ | ✅ |
| Private endpoint sur l'env | ❌ | ✅ |
| [[subnet-delegation|Délégation du subnet]] | ❌ Non délégué | ✅ `Microsoft.App/environments` |
| Coût fixe | Non | Non si on n'utilise que le profil *Consumption* |

```bicep
workloadProfiles: [
  { name: 'Consumption', workloadProfileType: 'Consumption' }
]
```

Indice : un env déployé avec une vieille API (ex. `2022-03-01`) sans `workloadProfiles` est du *Consumption only*.

Détails (profils, détection, migration) : [[container-apps-workload-profiles]] · facturation : [[container-apps-plan-consumption]].

## Contourner sans recréer

Quand l'env existant a un mauvais choix (ex. externe), on peut souvent **ajouter un nouvel env** à côté pour le besoin concerné plutôt que tout recréer (ex. [[collecteur-otel-central-acces-reseau|un env interne dédié au collecteur OTel]]).

## Voir aussi

- [[container-apps-environnement-externe-vs-interne]]
- [[vnet-subnets-adressage]]
- [[container-apps-workload-profiles]]
- [[subnet-delegation]]
