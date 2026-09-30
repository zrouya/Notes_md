---
tags: [azure, reseau, vnet]
---

# VNet Azure — le principe

Un **VNet (Virtual Network)** est un réseau privé que l'on dessine dans Azure : on choisit ses adresses IP, on le découpe en morceaux, on décide qui a le droit de parler à qui. C'est la brique réseau de base sur laquelle se branchent VM, APIM, App Service, bases de données, etc.

## L'analogie : le réseau d'un bureau

| Dans un bureau physique | Dans Azure |
|-------------------------|------------|
| Le réseau de l'entreprise | Le **VNet** |
| Un étage / un service séparé | Un **subnet** (sous-réseau) → [[vnet-subnets-adressage]] |
| La table du routeur (« pour aller là, passe par ici ») | La **route table** → [[vnet-routage-udr]] |
| Le pare-feu à l'entrée d'un étage | Le **NSG** → [[network-security-group-nsg-azure]] |
| Une ligne dédiée entre deux sites | Le **peering** / le **VPN** → [[vnet-peering]], [[vpn-gateway-expressroute]] |
| La box qui sort sur Internet | La **NAT Gateway** → [[sortie-internet-nat-gateway]] |

La différence : **il n'y a aucun câble ni switch**. Tout est simulé par logiciel sur les serveurs physiques d'Azure (SDN, *Software Defined Network*). On ne gère donc que des **règles**, jamais du matériel.

## Les 4 règles de base à retenir

1. **Un VNet = une région + un abonnement.** Il ne s'étend pas sur deux régions (on relie deux VNets à la place).
2. **Isolé par défaut.** Deux VNets ne se voient pas, même dans le même abonnement, tant qu'on ne les relie pas.
3. **Tout le monde se voit à l'intérieur.** Par défaut, toutes les machines de tous les subnets d'un VNet peuvent se joindre. Pour cloisonner, on ajoute des NSG.
4. **Le VNet est gratuit.** On paie ce qui s'y branche : gateways, firewall, NAT, trafic de peering.

## Ce qu'un VNet n'a pas (contrairement à un vrai LAN)

- Pas de **broadcast** ni de **multicast** : Azure répond lui-même aux requêtes [[Adress Resolution Protocol (ARP)|ARP]].
- Pas de [[serveur-dhcp|DHCP]] à gérer : chaque carte réseau reçoit son IP d'Azure, selon le subnet.
- Pas de `ping` garanti : l'ICMP est souvent bloqué, voir [[diagnostic-par-couche]].

## Prérequis réseau utiles

- [[Adresse IP]] et [[cidr-notation]] : lire `10.10.0.0/16`
- [[Sous-réseau]] : pourquoi et comment on découpe
- [[Routage réseau]] : table de routage, passerelle

## Voir aussi

- [[_index-reseau-vnet]] : parcours de lecture complet
- [[vnet-subnets-adressage]]
- [[hub-and-spoke]]
