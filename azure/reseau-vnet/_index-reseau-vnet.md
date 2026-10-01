---
tags: [index, azure, reseau, vnet]
---

# Réseau Azure (VNet) — Map of Content

Notes pour comprendre le réseau dans Azure. Chaque note suppose connues les précédentes : les lire **dans l'ordre** la première fois.

Bases réseau générales (IP, CIDR, routage, DNS) : voir [[_index-reseau|l'index Réseau]].

## Parcours de lecture

| # | Note | Ce qu'on y apprend |
|---|------|--------------------|
| 0 | [[cidr-notation]] · [[Sous-réseau]] | Prérequis : lire `10.10.0.0/16`, pourquoi découper |
| 1 | [[vnet-principe]] | Ce qu'est un VNet, l'analogie avec le réseau d'un bureau |
| 2 | [[vnet-subnets-adressage]] | Address space, subnets, IP réservées, chevauchement |
| 3 | [[vnet-routage-udr]] | Routes système, UDR, préfixe le plus long |
| 4 | [[filtrage-stateful-stateless]] | Prérequis au NSG : pourquoi on n'écrit que l'aller |
| 5 | [[network-security-group-nsg-azure]] | Règles, priorités, règles par défaut |
| 6 | [[nsg-service-tags-asg]] | Écrire des règles par nom plutôt que par IP |
| 7 | [[vnet-peering]] | Relier deux VNets ; **non-transitivité** |
| 8 | [[hub-and-spoke]] | L'architecture de référence |
| 9 | [[vpn-gateway-expressroute]] | Relier Azure et l'on-prem |
| 10 | [[service-endpoint-vs-private-endpoint]] | Rendre un PaaS privé |
| 11 | [[private-dns-zone]] | Le DNS qui fait marcher les private endpoints |
| 12 | [[vnet-integration-subnet-delegation]] | La sortie d'un PaaS vers le VNet ; entrée vs sortie |
| 13 | [[sortie-internet-nat-gateway]] | Sortir sur Internet avec une IP fixe |
| 14 | [[routage-asymetrique]] | Le piège du firewall qui ne voit que l'aller |

## Les pièges classiques, en une ligne

- **Plages qui se chevauchent** : impossible de relier les réseaux ensuite → [[vnet-subnets-adressage]]
- **Peering non transitif** : A↔B et B↔C ne donnent pas A↔C → [[vnet-peering]]
- **DNS des private endpoints** : ça marche par IP mais pas par nom → [[private-dns-zone]]
- **Aller et retour par des chemins différents** : timeouts → [[routage-asymetrique]]
- **NSG sur le subnet et sur la NIC** : il faut passer les deux → [[network-security-group-nsg-azure]]
- **`AllowVnetInBound` inclut les peerings** : on ouvre plus que prévu → [[nsg-service-tags-asg]]
- **VNet Integration ≠ application privée** : l'entrée reste publique → [[vnet-integration-subnet-delegation]]
- **Plus de sortie Internet implicite** sur les subnets récents → [[sortie-internet-nat-gateway]]

## Mémo : « je veux… → j'utilise »

| Je veux… | J'utilise |
|----------|-----------|
| cloisonner deux subnets | NSG |
| inspecter tout le trafic | Azure Firewall + UDR |
| relier deux VNets | Peering |
| relier le bureau | VPN Gateway / ExpressRoute |
| rendre un Storage/SQL privé | Private Endpoint + Private DNS Zone |
| qu'une App Service appelle du privé | VNet Integration |
| une IP de sortie fixe | NAT Gateway |
