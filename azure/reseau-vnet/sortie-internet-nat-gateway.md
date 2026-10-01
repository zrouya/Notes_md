---
tags: [azure, reseau, vnet, nat, internet]
---

# Sortie vers Internet depuis un VNet : NAT Gateway

Une machine en IP privée (`10.x`) ne peut pas parler directement à Internet : il faut qu'un équipement **remplace son IP privée par une IP publique** à la sortie. C'est le [[NAT (Network Address Translation)|NAT]], exactement comme la box internet d'une maison.

## Les façons de sortir

| Méthode | IP de sortie | Commentaire |
|---------|--------------|-------------|
| **NAT Gateway** sur le subnet | Fixe, la vôtre | ✅ Méthode recommandée |
| IP publique sur la VM | Fixe | La VM devient aussi joignable **en entrée** |
| Load Balancer + règles *outbound* | Fixe | Si un LB public existe déjà |
| Azure Firewall / NVA (via UDR) | Celle du firewall | Quand on veut **inspecter** la sortie |
| « Default outbound access » | Aléatoire, gérée par Microsoft | ⚠️ En cours de retrait |

## Le « default outbound access » disparaît

Historiquement, une VM sans rien de configuré sortait quand même sur Internet, par une IP choisie par Microsoft, **non fixe**. Microsoft retire progressivement ce comportement : les subnets récents sont **privés par défaut** (propriété de subnet `defaultOutboundAccess = false`).

Conséquence : **il faut prévoir une sortie explicite**, sinon plus d'accès Internet (mises à jour, appels d'API externes…).

## NAT Gateway en pratique

```
snet-app (10.10.1.0/24) ──> NAT Gateway ──> IP publique 20.50.x.x ──> Internet
```

- On l'associe à **un ou plusieurs subnets**. Elle prend alors le dessus sur les autres méthodes de sortie.
- **Sortie uniquement** : personne ne peut entrer par elle.
- **IP fixe** : pratique quand un partenaire demande « donnez-nous votre IP pour qu'on l'autorise dans notre firewall ».
- ~64 000 **ports SNAT** par IP publique : sous forte charge, on peut les épuiser. Voir [[ports-ephemeres-time-wait]].

## Voir aussi

- [[NAT (Network Address Translation)]]
- [[vnet-routage-udr]] : la route `0.0.0.0/0 → Internet`
- [[vnet-integration-subnet-delegation]]
