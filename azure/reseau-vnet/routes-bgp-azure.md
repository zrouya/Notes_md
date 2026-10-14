---
tags: [azure, reseau, vnet, routage, bgp, hybride]
---

# Routes BGP dans Azure

Routes qu'un VNet **apprend automatiquement de l'on-prem**, par la gateway VPN ou ExpressRoute qui échange ses routes en [[bgp-border-gateway-protocol|BGP]] avec les routeurs de l'entreprise.

> [!abstract] Portée : spécifique Azure
> Concept général : [[bgp-border-gateway-protocol]] (protocole) et [[routes-statiques-vs-dynamiques]] (une route BGP est une route dynamique).

## Le schéma

```
Routeurs on-prem  ── BGP ──  Gateway VPN / ExpressRoute (hub)
  annoncent 192.168.0.0/16     annonce la plage du hub
                               + celle des spokes qui utilisent la gateway
```

## Ce qu'il faut pour qu'un spoke les reçoive

| Où | Réglage |
|----|---------|
| Peering, côté hub | **Allow gateway transit** |
| Peering, côté spoke | **Use remote gateways** |
| Route table du subnet | **Propagate gateway routes** activé (`disableBgpRoutePropagation: false`, valeur par défaut) |

La plage du spoke est alors annoncée à l'on-prem dans l'autre sens.

## Où elles apparaissent

- Dans **Effective routes** d'une carte réseau, avec la source `VirtualNetworkGateway` et le next hop `VirtualNetworkGateway`.
- Pas dans la colonne *Route table* d'un subnet : celle-ci ne montre que les [[vnet-routage-udr|UDR]].

## Priorité

1. Le **préfixe le plus long** gagne.
2. À préfixe égal : **UDR > BGP > route système**.

## Pièges

- BGP ne relie **que l'on-prem et Azure**. Il ne fait jamais communiquer deux spokes : pour ça, il faut une UDR vers le firewall du hub, voir [[hub-and-spoke]].
- Une route BGP plus précise qu'une UDR **gagne** sur elle : l'on-prem peut contourner le firewall sans qu'on le voie. Désactiver *Propagate gateway routes* sur les subnets dont tout le trafic doit passer par le firewall.
- Un chemin BGP direct au retour, alors que l'aller passe par le firewall : [[routage-asymetrique]].

## Voir aussi

- [[vpn-gateway-expressroute]] : les deux types de gateway
- [[vnet-routage-udr]] · [[verifier-routage-subnet-azure]]
- [[vnet-peering-fonctionnement]]
