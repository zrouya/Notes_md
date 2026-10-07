---
tags: [azure, reseau, vnet, routage, diagnostic, portail]
---

# Vérifier le routage d'un subnet (peering, UDR)

Un VNet appairé au hub n'a **pas forcément d'UDR** : le peering n'injecte que des routes système. Pour savoir comment un subnet route réellement son trafic, vérifier dans cet ordre.

## Dans le portail

1. **Le peering** : *VNet* → **Peerings**. Vérifier l'état `Connected` et ouvrir le lien pour lire les options (*Allow forwarded traffic*, *Use remote gateways*).
2. **La route table du subnet** : *VNet* → **Subnets** → colonne **Route table**, ou ouvrir le subnet. `None` = aucune UDR.
3. **Le contenu de la route table** : *Route tables* → la table → **Routes** (préfixe, *Next hop type*, *Next hop IP*). L'onglet **Subnets** liste les subnets associés.
4. **Le next hop** : comparer *Next hop IP* avec l'**IP privée du firewall** (*Azure Firewall* → *Overview* → *Private IP*).
5. **La table réellement appliquée** : *VM* → *Networking* → la carte réseau → **Effective routes**. Montre ensemble routes système, peering, BGP et UDR, avec la colonne *Source* (`Default`, `User`, `VirtualNetworkGateway`).
6. **Un test de chemin** : *Network Watcher* → **Next hop**, avec une VM source et une IP destination.

Les étapes 5 et 6 exigent une **carte réseau visible**. Pour un PaaS intégré au VNet (Container Apps, App Service) qui n'en expose pas : créer une petite VM de test dans un autre subnet **associé à la même route table**.

## En CLI

```bash
# Route table associée au subnet (vide = aucune UDR)
az network vnet subnet show -g <rg> --vnet-name <vnet> -n <subnet> --query routeTable.id -o tsv

# Routes de cette table
az network route-table route list -g <rg-rt> --route-table-name <rt> -o table

# Peerings du VNet et leurs options
az network vnet peering list -g <rg> --vnet-name <vnet> \
  --query "[].{nom:name, etat:peeringState, forwarded:allowForwardedTraffic, remoteGw:useRemoteGateways}" -o table

# Routes effectives d'une carte réseau
az network nic show-effective-route-table -g <rg> -n <nic> -o table
```

## Lire le résultat

| Constat | Conséquence |
|---------|-------------|
| Peering `Connected`, pas de route table | Le subnet joint le hub, **pas les autres spokes** |
| Route `0.0.0.0/0` ou `10.0.0.0/8` → `VirtualAppliance` | Le trafic passe par le firewall : spoke à spoke possible si le firewall l'autorise |
| Routes `VirtualNetworkGateway` présentes | *Use remote gateways* actif : l'on-prem est joignable |
| UDR d'un seul côté | Risque de [[routage-asymetrique]] |

Certains services ignorent l'UDR posée sur leur subnet (ex. Container Apps en *Consumption only*, voir [[container-apps-workload-profiles]]) : la route table existe, mais n'a pas d'effet.

## Voir aussi

- [[vnet-routage-udr]] · [[vnet-peering-fonctionnement]]
- [[hub-and-spoke]]
