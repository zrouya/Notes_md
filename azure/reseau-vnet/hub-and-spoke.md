---
tags: [azure, reseau, vnet, architecture]
---

# Architecture hub-and-spoke

Topologie réseau Azure la plus courante : un VNet central (**hub**) porte les services partagés, et des VNets satellites (**spokes**, les « rayons » d'une roue) portent les applications. Tous les spokes sont appairés au hub, jamais entre eux.

C'est la version cloud de la [[Topologie réseau en étoile]].

## Schéma

```
                    Réseau on-prem (bureaux, datacenter)
                              │
                      VPN / ExpressRoute
                              │
              ┌───────────────┴───────────────┐
              │  HUB  10.0.0.0/16             │
              │  - VPN Gateway                │
              │  - Azure Firewall 10.0.1.4    │
              │  - DNS Private Resolver       │
              │  - Bastion                    │
              └───────┬───────────────┬───────┘
               peering│               │peering
         ┌────────────┴──┐       ┌────┴──────────┐
         │ SPOKE app-1   │       │ SPOKE app-2   │
         │ 10.1.0.0/16   │       │ 10.2.0.0/16   │
         └───────────────┘       └───────────────┘
```

## Pourquoi cette forme

- **Mutualiser** ce qui coûte cher ou demande de l'expertise (firewall, gateway, DNS) : un seul exemplaire.
- **Un point de contrôle unique** : tout trafic sensible passe par le firewall du hub, il est donc inspecté et journalisé.
- **Isoler les applications** : chaque équipe a son spoke, un incident reste contenu.

## Comment un spoke parle à un autre spoke

Le peering n'étant [[vnet-peering#Le point clé : le peering n'est PAS transitif|pas transitif]], le spoke 1 ne voit pas le spoke 2. On fait relayer le trafic par le firewall :

1. **UDR dans chaque spoke** : `10.0.0.0/8 → VirtualAppliance 10.0.1.4`, ou `0.0.0.0/0` pour tout inspecter. Voir [[vnet-routage-udr]].
2. **Allow forwarded traffic** activé sur les peerings.
3. **Règle sur le firewall** : autoriser `10.1.0.0/16 → 10.2.0.0/16` sur les ports voulus.

## Comment un spoke joint l'on-prem

Par la gateway du hub : *Allow gateway transit* côté hub, *Use remote gateways* côté spoke.

## Alternative

**Azure Virtual WAN** : un hub géré par Microsoft, intéressant avec beaucoup de sites et de régions. Plus simple à exploiter, mais moins de contrôle fin.

## Voir aussi

- [[vnet-peering]] · [[vpn-gateway-expressroute]]
- [[routage-asymetrique]] : le piège n°1 de cette architecture
- [[private-dns-zone]] : le DNS se centralise aussi dans le hub
