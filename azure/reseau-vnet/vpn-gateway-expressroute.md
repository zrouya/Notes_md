---
tags: [azure, reseau, vnet, vpn, hybride]
---

# Relier Azure à l'on-prem : VPN Gateway et ExpressRoute

« On-prem » (*on-premises*) désigne les réseaux de l'entreprise hors du cloud : bureaux, datacenter. Deux façons de les relier à un VNet en IP privée.

## VPN Gateway : un tunnel chiffré sur Internet

Un [[Virtual Private Network (VPN)|VPN]] crée un **tunnel** : les paquets privés sont chiffrés ([[Protocole IPSec|IPsec]]) puis emballés dans des paquets publics qui traversent Internet.

```
Bureau 192.168.0.0/16                              VNet 10.0.0.0/16
[routeur/firewall] ══ tunnel IPsec sur Internet ══ [VPN Gateway]
   IP publique A                                     IP publique B
```

| Mode | Pour qui |
|------|----------|
| **Site-to-Site (S2S)** | Un site entier : routeur du bureau ↔ Azure |
| **Point-to-Site (P2S)** | Un poste individuel (télétravail) ↔ Azure |

- Se déploie dans un subnet nommé obligatoirement `GatewaySubnet`.
- Rapide à monter, peu coûteux, mais la performance dépend d'Internet.

## ExpressRoute : une ligne privée dédiée

Un **circuit privé** fourni par un opérateur télécom, entre le datacenter et Microsoft. **Il ne passe pas par Internet.**

- Débit garanti, latence stable, jusqu'à plusieurs dizaines de Gbit/s.
- Plus cher, et plusieurs semaines de mise en place avec l'opérateur.
- ⚠️ **Non chiffré par défaut** : privé ne veut pas dire chiffré. Ajouter IPsec ou MACsec si besoin.

## Comparaison

| | VPN Gateway | ExpressRoute |
|--|-------------|--------------|
| Chemin | Internet | Ligne opérateur |
| Chiffrement | Oui (IPsec) | Non par défaut |
| Performance | Variable | Garantie |
| Délai de mise en place | Heures | Semaines |
| Coût | € | €€€ |

Souvent on combine les deux : **ExpressRoute en principal, VPN en secours**.

## Comment les routes sont échangées

Via **BGP**, un protocole par lequel deux routeurs s'annoncent mutuellement « voici les réseaux que je sais joindre ». Les plages on-prem apparaissent alors dans les routes du VNet. Voir [[vnet-routage-udr]].

## Rappel

Les plages on-prem et Azure **ne doivent pas se chevaucher**, voir [[vnet-subnets-adressage]].

## Voir aussi

- [[hub-and-spoke]] : la gateway vit dans le hub
- [[Wide Area Network (WAN)]]
