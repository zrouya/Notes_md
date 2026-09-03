---
tags: [reseau, ip, subnet, azure]
---

# Sous-réseau

Découpage d'un réseau IP en segments plus petits, en empruntant des bits à la partie hôte pour agrandir la partie réseau.

## Pourquoi découper

- **Isolation** — appliquer des règles de filtrage distinctes par segment (NSG Azure, security groups)
- **Routage** — deux adresses du même sous-réseau se joignent directement ; sinon il faut passer par la [[Passerelle réseau|passerelle]]
- **Limiter la diffusion** — le broadcast ne franchit pas la frontière d'un sous-réseau

## Le test « même sous-réseau ? »

Une machine applique le [[Masque de sous-réseau|masque]] à sa propre adresse **et** à la destination. Résultats identiques → livraison directe sur le lien ; différents → envoi à la passerelle par défaut.

C'est ce test, et rien d'autre, qui décide si un paquet sort du réseau local.

## Exemple

```
Reseau           10.0.0.0/16      (65 536 adresses)
  ├─ subnet-app  10.0.1.0/24      (256)
  ├─ subnet-data 10.0.2.0/24      (256)
  └─ subnet-gw   10.0.3.0/27      (32)
```

Les sous-réseaux d'un même VNet **ne doivent pas se chevaucher**, et le VNet lui-même ne doit pas chevaucher un réseau avec lequel il sera appairé — un chevauchement rend le peering impossible **après coup**, sans possibilité de redimensionner sans tout recréer.

## Spécificité Azure

Azure réserve **5 adresses par subnet** : les 4 premières (réseau, passerelle, 2 pour le DNS) et la dernière (broadcast).

| Préfixe | Total | Utilisables sur Azure |
|---------|-------|-----------------------|
| `/29` | 8 | **3** |
| `/28` | 16 | 11 |
| `/24` | 256 | 251 |

`/29` est le plus petit subnet autorisé.

## Voir aussi

- [[cidr-notation]] · [[Masque de sous-réseau]]
- [[Routage réseau]] · [[Adresse IP]]
