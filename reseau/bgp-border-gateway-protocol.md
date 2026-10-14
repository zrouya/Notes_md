---
tags: [reseau, routage, bgp, generique]
---

# BGP (Border Gateway Protocol)

Protocole par lequel deux routeurs **s'échangent leurs routes** : chacun annonce à l'autre les plages d'adresses qu'il sait joindre. Les routes apprises ainsi sont des **routes dynamiques**.

> [!abstract] Portée : notion générique (réseau)
> Indépendante de tout fournisseur cloud. Déclinaison Azure : [[routes-bgp-azure]].

## Exemple

```
Routeur A (site Paris)  ── session BGP (TCP 179) ──  Routeur B (datacenter)
  annonce : 192.168.0.0/16                             annonce : 10.0.0.0/16
  apprend : 10.0.0.0/16 via B                          apprend : 192.168.0.0/16 via A
```

Chaque routeur ajoute les plages apprises à sa [[Routage réseau|table de routage]], avec l'autre routeur comme prochain saut.

## Détails

- **Système autonome (AS)** : un réseau géré par une même organisation, identifié par un numéro (ASN). Les ASN `64512-65534` sont privés, comme les [[plages-ip-reservees|plages IP privées]].
- **eBGP** : entre deux AS différents (entreprise ↔ opérateur, entreprise ↔ cloud). **iBGP** : à l'intérieur d'un même AS.
- Une **session BGP** est une connexion TCP sur le port **179** entre deux voisins (*peers*) configurés explicitement.
- BGP choisit les chemins selon des **attributs** (longueur du chemin d'AS, préférences locales), pas selon la vitesse des liens.
- C'est le protocole qui fait tenir **Internet** : les opérateurs s'y annoncent les plages publiques (voir [[ip-publique-vs-privee]]). On l'utilise aussi entre deux réseaux privés, par exemple sur un [[Virtual Private Network (VPN)|VPN]] site à site.

## Pourquoi c'est utile

- Quand un site ajoute une plage, l'autre côté l'apprend **sans modification manuelle**.
- Si un lien tombe, la route est retirée et le trafic bascule sur un autre lien annonçant la même plage.

## Pièges

- BGP annonce ce qu'on lui dit d'annoncer : une plage mal filtrée peut **détourner du trafic** (fuite de routes).
- Deux réseaux qui annoncent la même plage (chevauchement) rendent le routage ambigu.

## Voir aussi

- [[routes-statiques-vs-dynamiques]] : BGP est un protocole de routage dynamique
- [[Routage réseau]] · [[Routeurs réseaux]]
- [[routes-bgp-azure]] : comment Azure apprend les routes de l'on-prem
- [[acheminement-ip-publique-privee]]
