---
tags: [azure, reseau, vnet, paas, subnet]
---

# Délégation de subnet

La **délégation** réserve un subnet à **un service Azure PaaS précis**. Elle donne à ce service le droit d'y déployer et d'y gérer lui-même son infrastructure (cartes réseau, nœuds, policies réseau).

## Syntaxe

```bicep
resource subnet 'Microsoft.Network/virtualNetworks/subnets@2023-09-01' = {
  parent: vnet
  name: 'snet-aca'
  properties: {
    addressPrefix: '10.0.4.0/27'
    routeTable: { id: routeTable.id }            // les UDR restent applicables
    networkSecurityGroup: { id: nsg.id }
    delegations: [
      { name: 'aca', properties: { serviceName: 'Microsoft.App/environments' } }
    ]
  }
}
```

```bash
# Lister les délégations possibles dans une région
az network vnet subnet list-available-delegations -l francecentral -o table
# Déléguer un subnet existant
az network vnet subnet update -g <rg> --vnet-name <vnet> -n <snet> --delegations Microsoft.App/environments
```

## Principaux services qui délèguent

| Service | `serviceName` |
|---|---|
| Container Apps (*Workload profiles*) | `Microsoft.App/environments` |
| App Service / Functions ([[vnet-integration-subnet-delegation\|VNet Integration]]) | `Microsoft.Web/serverFarms` |
| Container Instances | `Microsoft.ContainerInstance/containerGroups` |
| SQL Managed Instance | `Microsoft.Sql/managedInstances` |
| PostgreSQL / MySQL Flexible Server | `Microsoft.DBforPostgreSQL/flexibleServers`, `Microsoft.DBforMySQL/flexibleServers` |
| Azure NetApp Files | `Microsoft.Netapp/volumes` |

## Règles

- **Un seul service** par subnet délégué.
- Le subnet devient **dédié** : on ne peut plus y placer de VM ni de private endpoint.
- Impossible de déléguer un subnet qui contient déjà d'autres ressources.
- NSG et route table restent associables. Le service peut exiger certaines règles (voir sa doc).
- La délégation est une permission accordée, pas une création : le service crée ses ressources au déploiement.

## Pièges

- **Suppression bloquée** : le service pose un *Service Association Link* sur le subnet. Tant que la ressource PaaS existe (ou si son nettoyage a échoué), le subnet ne peut être ni supprimé ni dé-délégué.
- **Ne pas déléguer quand le service ne le demande pas.** Exemple : un env Container Apps *Consumption only* attend un subnet **non** délégué.
- **Non délégué ≠ partageable** : certains services exigent un subnet dédié sans délégation, uniquement par convention (rien ne bloque techniquement). Voir [[container-apps-consumption-only-architecture]].
- **Dimensionner pour le scale-out maximal** : chaque instance ou nœud consomme une IP du subnet, en plus des 5 IP réservées par Azure ([[vnet-subnets-adressage]]).

## Voir aussi

- [[vnet-integration-subnet-delegation]]
- [[container-apps-workload-profiles]]
- [[vnet-subnets-adressage]]
