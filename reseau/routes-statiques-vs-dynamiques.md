---
tags: [reseau, routage, generique]
---

# Routes statiques vs routes dynamiques

Une table de routage mélange des routes **écrites à la main** (statiques), des routes **apprises d'autres routeurs** (dynamiques) et des routes **connectées** (les réseaux directement branchés sur le routeur).

> [!abstract] Portée : notion générique (réseau)
> Déclinaisons Azure : la route statique devient l'[[vnet-routage-udr|UDR]], la route dynamique devient la [[routes-bgp-azure|route BGP]], la route connectée devient la route système.

## Exemple (routeur Linux)

```bash
# Route statique : pour joindre 10.3.0.0/16, passer par le firewall 10.0.1.4
ip route add 10.3.0.0/16 via 10.0.1.4

# Afficher la table : connectées, statiques et dynamiques (proto bgp)
ip route show
```

## Comparaison

| | Statique | Dynamique |
|--|----------|-----------|
| Qui la crée | Un administrateur | Un protocole ([[bgp-border-gateway-protocol\|BGP]], OSPF…) |
| Réaction à une panne | Aucune : la route reste | Retirée, le trafic bascule |
| Usage typique | Forcer un passage (firewall), petit réseau | Réseaux étendus, liens multiples, Internet |
| Risque | Oubli, route obsolète | Fuite ou mauvaise annonce |

## Choisir entre plusieurs routes

1. **Le préfixe le plus long gagne** : `10.3.1.0/24` bat `10.3.0.0/16`, qui bat `0.0.0.0/0` (voir [[cidr-notation]]).
2. À préfixe égal, la **distance administrative** départage : une route statique est généralement préférée à une route dynamique.

`0.0.0.0/0` est la **route par défaut** : elle correspond à tout, mais elle est la moins précise.

## Voir aussi

- [[Routage réseau]] · [[Passerelle réseau]]
- [[bgp-border-gateway-protocol]]
- [[vnet-routage-udr]] · [[routes-bgp-azure]] : les équivalents Azure
