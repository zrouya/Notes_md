---
tags: [azure, container-apps, aks, reseau, vnet, subnet, diagnostic]
---

# Container Apps *Consumption only* : architecture sous-jacente et IP du subnet

Un environnement *Consumption only* repose sur un **cluster AKS managé et invisible**, en réseau **Azure CNI** : chaque pod reçoit une vraie IP du subnet. C'est ce qui explique la consommation d'IP et le subnet minimum en **/23**.

## Ce qu'il y a derrière l'environnement

| Élément | Où | Rôle |
|---|---|---|
| `aks-systempool-…-vmss` | Resource group d'infra `MC_…` | Plateforme Microsoft (Envoy, KEDA, Dapr, agents). Plusieurs nœuds pour la HA |
| `aks-userpool1-…-vmss` | Idem | Les **réplicas des apps** ; scale-out et scale-in selon la charge |
| Load balancer | Idem | Frontend **privé** dans le subnet si env interne, IP **publique** si externe |

## Comment le subnet est consommé

- Chaque nœud **pré-réserve un lot d'IP** (IP du nœud + IP pour ses futurs pods), qu'elles soient utilisées ou non. Ordre de grandeur : **~30 IP par nœud**, variable selon le pool (max pods fixé par Microsoft).
- Le pool système consomme une **part fixe**, même avec zéro application.
- Le nombre d'IP suit le **nombre de nœuds**, pas le nombre d'apps ni de réplicas.
- **Calcul de marge** : /23 = 512 − 5 réservées = 507 utilisables. Libres ÷ IP par nœud utilisateur ≈ nœuds ajoutables. Garder de la marge pour les nœuds temporaires (*surge*) des mises à jour.

## Le subnet est dédié… par convention

- Le subnet **n'est pas délégué** : les NIC des nœuds s'y branchent comme celles de VM classiques.
- Rien ne verrouille donc le subnet, mais Microsoft impose qu'il soit **réservé à l'env**. D'autres ressources volent des IP (risque d'échec du scaling) et héritent du NSG imposé.
- En *Workload profiles*, la [[subnet-delegation|délégation]] rend le subnet dédié **par mécanisme**.

## Diagnostiquer (PowerShell)

```powershell
# Résumé : délégation, Service Association Links, nombre d'IP prises
az network vnet subnet show -g <rg> --vnet-name <vnet> -n <snet> --query '{delegations:delegations, sal:serviceAssociationLinks, ipConfigs:length(ipConfigurations || `[]`)}'

# IP par nœud (VMSS + n° d'instance)
$s = az network vnet subnet show -g <rg> --vnet-name <vnet> -n <snet> | ConvertFrom-Json
$s.ipConfigurations.id | ForEach-Object {
  if ($_ -match 'virtualMachineScaleSets/([^/]+)/virtualMachines/(\d+)') { "$($matches[1]) #$($matches[2])" } else { $_ }
} | Group-Object | Select-Object Count, Name
```

- **Quotes** : en PowerShell comme en bash, mettre `--query` entre **guillemets simples** pour que le littéral JMESPath `` `[]` `` passe tel quel. `\`` ne marche qu'en bash entre guillemets doubles.
- Dans un VMSS, toutes les instances ont une NIC **de même nom** : il faut extraire le n° d'instance pour compter par nœud.
- Aucune IP de load balancer dans le subnet → env probablement **externe**.
- **N° d'instance élevé** (#300+) : le VMSS incrémente sans réutiliser. Cela trahit de nombreux cycles de scale ou de mises à jour, ce qui est normal pour le pool utilisateur.

## Voir aussi

- [[container-apps-workload-profiles]]
- [[container-apps-environnement-externe-vs-interne]]
- [[subnet-delegation]] · [[vnet-subnets-adressage]]
- [[azure-kubernetes-service]]
