---
tags: [azure, reseau, vnet, dns]
---

# DNS d'un VNet : DNS Azure (168.63.129.16) ou serveurs personnalisés

Toutes les ressources d'un VNet utilisent **le même résolveur DNS**, fixé par le réglage *DNS servers* du VNet. Par défaut, c'est le DNS fourni par Azure, à l'adresse virtuelle `168.63.129.16`.

> [!abstract] Portée : spécifique Azure
> Concept général : un réglage de [[dns-roles-resolveur-autorite|résolveur récursif]] distribué par DHCP.

## Le réglage *DNS servers* du VNet

| Valeur | Résolveur utilisé par les ressources |
|--------|--------------------------------------|
| *Default (Azure-provided)* | `168.63.129.16` |
| *Custom* + une ou plusieurs IP | Ces serveurs : contrôleurs de domaine, *inbound endpoint* d'un [[dns-private-resolver-azure\|DNS Private Resolver]], DNS proxy d'Azure Firewall… |

```bash
az network vnet show -g <rg> -n <vnet> --query dhcpOptions.dnsServers
```

- Les PaaS reliés au VNet suivent ce réglage : Web App avec [[vnet-integration-subnet-delegation|VNet Integration]], environnement Container Apps, AKS…
- Le réglage est distribué par DHCP : une VM ne le voit qu'après **redémarrage** ou renouvellement du bail.
- Il se règle **par VNet**, pas par subnet. Le peering ne le propage pas.

## Ce que sait faire 168.63.129.16

- Résoudre les **noms publics** d'Internet et des services Azure.
- Résoudre les **[[private-dns-zone|Private DNS Zones]] liées au VNet d'où part la question**, et seulement celles-là.
- Résoudre les noms des VMs du VNet (suffixe `internal.cloudapp.net`), ou d'une zone privée avec auto-enregistrement.

## Ses limites

- **Joignable uniquement depuis Azure.** Un serveur on-prem ne peut pas l'interroger, même par VPN ou ExpressRoute : il faut un [[dns-private-resolver-azure|Private Resolver]].
- Pas de redirection conditionnelle propre. Pour joindre une zone d'entreprise, il faut un *forwarding ruleset* (Private Resolver) ou des serveurs personnalisés.
- Cette adresse sert aussi aux sondes de santé et à l'agent de plateforme : ne jamais la bloquer dans un NSG ou un firewall local.

## Serveurs personnalisés : la règle d'or

Avec *Custom*, ces serveurs deviennent le seul résolveur du VNet. Ils doivent donc savoir répondre à **tout** ce dont les ressources ont besoin :
- les zones de l'entreprise ;
- les zones privées Azure (`privatelink.*`), en redirigeant vers un résolveur qui voit ces zones ;
- les noms publics, dont ceux utilisés par la plateforme Azure elle-même. Sinon, un PaaS peut se dégrader sans message clair.

## Voir aussi

- [[dns-hybride-azure]] : comment s'articulent DNS Azure et DNS d'entreprise
- [[private-dns-zone]] · [[dns-private-resolver-azure]]
- [[dns-roles-resolveur-autorite]] : le concept général
