---
tags: [azure, reseau, vnet, dns, hybride]
---

# Azure DNS Private Resolver

Service managé qui fait le **pont entre le DNS d'Azure et un DNS extérieur** (on-prem, autre cloud). Il remplace les VMs DNS qu'on déployait autrefois dans le hub pour cet usage.

> [!abstract] Portée : spécifique Azure
> Concept général : [[dns-redirection-conditionnelle]]. L'inbound endpoint est une cible de redirection, le ruleset est un ensemble de redirections conditionnelles.

## Les deux sens

| Composant | Sens | Rôle |
|-----------|------|------|
| **Inbound endpoint** | Extérieur → Azure | Une IP privée dans le VNet. Le DNS on-prem y redirige les zones Azure privées. Le resolver les résout avec `168.63.129.16`, donc avec les zones liées **à son propre VNet**. |
| **Outbound endpoint** + **forwarding ruleset** | Azure → extérieur | Règles « pour tel domaine, demander à telles IP ». Le ruleset est lié aux VNets qui doivent en bénéficier. |

```
On-prem  ──redirige *.privatelink.*──►  Inbound endpoint (10.0.5.4)  ──►  zones privées liées au hub

VNet lié au ruleset ─► 168.63.129.16 ─► règle « corp.local → 192.168.1.10 » ─► Outbound endpoint ─► DNS on-prem
```

## Mise en place

- Déployé dans un VNet, en général le **hub**.
- Un subnet dédié par endpoint, **délégué** à `Microsoft.Network/dnsResolvers` (voir [[subnet-delegation]]), `/28` minimum.
- Les [[private-dns-zone|Private DNS Zones]] à rendre visibles depuis l'extérieur doivent être **liées au VNet du resolver**.

## Pièges

- Un ruleset ne s'applique qu'aux VNets **liés** au ruleset **et** qui utilisent le DNS Azure par défaut. Un VNet en DNS personnalisé l'ignore.
- Les cibles d'une règle doivent être **routées** depuis le VNet de l'outbound endpoint, et le port 53 ouvert (UDP et TCP) sur les firewalls.
- Une zone liée au VNet d'un spoke, mais pas à celui du resolver, est **invisible** via l'inbound endpoint.

## Alternatives

| Option | Commentaire |
|--------|-------------|
| VMs DNS (souvent des contrôleurs de domaine) dans le hub | Historique, à exploiter et à patcher soi-même |
| Azure Firewall en **DNS proxy** | Le firewall reçoit le DNS des spokes et le transmet ; nécessaire pour filtrer par FQDN dans ses règles réseau |

## Voir aussi

- [[dns-hybride-azure]] · [[vnet-dns-azure-168-63-129-16]]
- [[hub-and-spoke]] · [[vpn-gateway-expressroute]]
- [[dns-redirection-conditionnelle]] : le concept général
