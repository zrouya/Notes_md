---
tags: [azure, reseau, vnet, peering]
---

# VNet Peering

Le **peering** relie deux VNets pour que leurs machines communiquent en IP privée, comme s'ils ne formaient qu'un réseau. Le trafic reste sur le réseau interne de Microsoft (*backbone*) et ne passe jamais par Internet.

## Ce qu'il faut savoir

- **Deux liens à créer** : un de A vers B, et un de B vers A. Tant que les deux ne sont pas en état `Connected`, rien ne passe.
- **Inter-régions possible** (*global peering*), et inter-abonnements.
- **Pas de chevauchement d'adresses** entre les deux VNets, sinon le peering est refusé. Voir [[vnet-subnets-adressage#Le piège n°1 : le chevauchement]].
- **Payant au Go** échangé, dans chaque sens.
- Aucun équipement à gérer : pas de gateway, pas de goulet d'étranglement.

## Le point clé : le peering n'est PAS transitif

```
   A  <──peering──>  B  <──peering──>  C

   A ↔ B  OK
   B ↔ C  OK
   A ↔ C  NON   (B ne fait pas relais)
```

Un peering ne relie **que les deux VNets qu'il connecte**. Pour que A parle à C, deux options :

1. créer un peering direct A ↔ C : ça devient vite ingérable (maillage complet) ;
2. mettre un **firewall ou un routeur dans B** et des UDR qui envoient le trafic vers lui. C'est le modèle [[hub-and-spoke]].

## Les options d'un lien de peering

| Option | Signification |
|--------|---------------|
| *Allow virtual network access* | Autoriser le trafic entre les deux VNets (activé par défaut) |
| *Allow forwarded traffic* | Accepter du trafic **relayé** par une appliance de l'autre VNet, donc pas originaire de lui |
| *Allow gateway transit* | (côté hub) Prêter ma VPN Gateway à l'autre VNet |
| *Use remote gateways* | (côté spoke) Utiliser la gateway du VNet d'en face pour joindre l'on-prem |

## Rappel NSG

Une fois le peering établi, le tag `VirtualNetwork` inclut le VNet d'en face : la règle par défaut `AllowVnetInBound` **laisse tout passer**. Cloisonner si nécessaire, voir [[network-security-group-nsg-azure]].

## Voir aussi

- [[hub-and-spoke]]
- [[vnet-routage-udr]]
- [[vpn-gateway-expressroute]]
