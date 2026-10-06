---
tags: [azure, container-apps, reseau, vnet, architecture, bicep, terraform]
---

# Container Apps Environment : Consumption only vs Workload profiles

Il existe **deux types** d'environnement Container Apps. *Workload profiles* est le type actuel, recommandé par Microsoft même si l'on ne fait que du serverless. *Consumption only* est le type historique (« v1 »).

## Comparatif

| | **Consumption only** (legacy) | **Workload profiles** |
|---|---|---|
| Profils disponibles | Serverless uniquement | Profil `Consumption` + profils Dedicated optionnels |
| Taille mini du subnet | **/23** | **/27** |
| [[subnet-delegation\|Délégation du subnet]] | ❌ Ne pas déléguer | ✅ Obligatoire : `Microsoft.App/environments` |
| [[vnet-routage-udr\|UDR]] (route vers un firewall) | ❌ Non respectées | ✅ |
| [[sortie-internet-nat-gateway\|NAT Gateway]] | ❌ | ✅ |
| Private endpoint sur l'env | ❌ | ✅ |
| Coût fixe | Non | Non avec le seul profil Consumption ; frais de gestion dès qu'un profil Dedicated est utilisé |

⚠️ En **hub-and-spoke avec firewall**, seul *Workload profiles* garantit que la sortie passe par le firewall. Il faut alors autoriser sur le firewall les FQDN requis par la plateforme (MCR, registre, Entra ID…).

## Les profils

| Profil | Nature | Facturation |
|---|---|---|
| `Consumption` | Serverless, max 4 vCPU / 8 GiB par réplica | À la seconde, voir [[container-apps-plan-consumption]] |
| `D4`…`D32` | Dedicated, usage général (1 vCPU : 4 GiB) | Par nœud, avec un nombre de nœuds min/max |
| `E4`…`E32` | Dedicated, optimisé mémoire (1 vCPU : 8 GiB) | Par nœud |
| `NC…-A100` / `Consumption-GPU-…` | GPU (dédié ou serverless) | Selon le profil |

Chaque app choisit son profil avec `workloadProfileName`.

## Syntaxe

```bicep
// API >= 2023-05-01 (GA des workload profiles)
resource env 'Microsoft.App/managedEnvironments@2024-03-01' = {
  properties: {
    vnetConfiguration: { infrastructureSubnetId: subnetId, internal: true }
    workloadProfiles: [
      { name: 'Consumption', workloadProfileType: 'Consumption' }
      // { name: 'd4', workloadProfileType: 'D4', minimumCount: 1, maximumCount: 3 }
    ]
  }
}
// côté app : properties.workloadProfileName: 'Consumption'
```

```hcl
# Terraform azurerm : sans bloc workload_profile => Consumption only
workload_profile {
  name                  = "Consumption"
  workload_profile_type = "Consumption"
}
```

## Identifier le type d'un env existant

```bash
az containerapp env show -n <env> -g <rg> --query "properties.workloadProfiles"
```

- `null` ou `[]` : **Consumption only**.
- Dans le code, une API antérieure à `2022-10-01` ne sait pas créer de workload profiles : l'env est forcément *Consumption only*.

## Migrer

Le type d'environnement est **immuable** : pas de conversion sur place.
1. Créer un **nouveau subnet** (/27 ou plus, délégué), car l'ancien n'est ni délégué ni à la bonne taille.
2. Créer le nouvel env *Workload profiles* et redéployer les apps dedans.
3. Basculer le trafic (DNS, App Gateway…), puis supprimer l'ancien env.

## Voir aussi

- [[container-apps-choix-irreversibles]]
- [[container-apps-environnement-externe-vs-interne]]
- [[subnet-delegation]]
