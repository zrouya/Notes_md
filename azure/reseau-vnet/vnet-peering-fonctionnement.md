---
tags: [azure, reseau, vnet, peering, sdn]
---

# VNet Peering — fonctionnement technique

Un peering n'est **ni un tunnel, ni une gateway, ni un équipement**. C'est une configuration du réseau logiciel (SDN) d'Azure : le VNet n'existe pas physiquement, il est appliqué sur chaque serveur physique qui héberge les ressources.

## Plan de contrôle : des routes injectées

Une fois le peering `Connected`, Azure ajoute dans les **routes effectives** de chaque subnet la plage du VNet d'en face :

```
Destination      Next hop type       Source
10.0.0.0/16      VNetPeering         Default    # VNet appairé, même région
10.5.0.0/16      VNetGlobalPeering   Default    # VNet appairé, autre région
```

Ce sont des **routes système**, pas des UDR. Seule la plage du VNet **directement** appairé est injectée, d'où la [[vnet-peering#Le point clé : le peering n'est PAS transitif|non-transitivité]].

## Plan de données : d'hôte à hôte

- Sur chaque hôte physique, le switch virtuel du SDN intercepte le paquet, applique les [[network-security-group-nsg-azure|NSG]], l'**encapsule** et l'envoie **directement à l'hôte de destination** par le backbone Microsoft.
- **Aucun saut intermédiaire** : même latence qu'à l'intérieur d'un VNet.
- **Pas de limite de débit propre au peering** : la limite est celle des VMs.
- Trafic **non chiffré** par défaut (réseau privé Microsoft). Option *VNet encryption* sur certaines tailles de VM.

## Cycle de vie d'un lien

| État | Signification |
|------|---------------|
| `Initiated` | Un seul des deux liens (A→B) est créé |
| `Connected` | Les deux liens existent : le trafic passe |
| `Disconnected` | L'autre côté a été supprimé : supprimer et recréer |

Après un **agrandissement de l'address space** d'un des VNets, le peering affiche *Remote sync required* : lancer l'action **Sync** des deux côtés pour propager la nouvelle plage.

## Ce que le peering ne fait pas

- **Il ne crée aucune UDR.** Aller plus loin que le hub (un autre spoke, via le firewall) demande une route table posée à part. Voir [[verifier-routage-subnet-azure]].
- **Il ne partage pas le DNS.** Chaque [[private-dns-zone]] doit être liée aux VNets qui la résolvent, ou les VNets utilisent un DNS commun (DNS Private Resolver du hub).
- **Il ne filtre rien.** Le tag `VirtualNetwork` inclut les VNets appairés. Voir [[nsg-service-tags-asg]].
- **Il ne fait pas transiter vers l'on-prem** sans *Allow gateway transit* (hub) + *Use remote gateways* (spoke). Avec ces options, les routes on-prem apprises en BGP arrivent dans le spoke (next hop `VirtualNetworkGateway`) et la plage du spoke est annoncée à l'on-prem.

## Voir aussi

- [[vnet-peering]] · [[hub-and-spoke]]
- [[vnet-routage-udr]] · [[routage-asymetrique]]
- [[vpn-gateway-expressroute]]
