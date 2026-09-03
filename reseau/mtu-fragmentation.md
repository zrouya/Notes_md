---
tags: [reseau, ip, vpn, kubernetes, diagnostic]
---

# MTU et fragmentation

La **MTU** (Maximum Transmission Unit) est la taille max de charge utile qu'un lien accepte : **1500 o** en Ethernet standard (trame de 1518 o au total).

## Valeurs de référence

| Contexte | MTU utile |
|----------|-----------|
| Ethernet standard | 1500 |
| PPPoE (ADSL) | 1492 |
| Tunnel VPN / IPsec / WireGuard | 1400 environ |
| Overlay Kubernetes (VXLAN) | 1450 |
| Jumbo frames (datacenter) | 9000 |

## Le symptôme à reconnaître

> La connexion s'établit, les petites requêtes passent, **les gros transferts se figent**.

C'est presque toujours la MTU. Mécanisme :

1. Un tunnel ajoute son propre en-tête → réduit la MTU utile.
2. Un paquet trop gros devrait déclencher un ICMP `Fragmentation Needed` vers l'émetteur.
3. Un firewall trop zélé **bloque l'ICMP** → l'émetteur ne l'apprend jamais (*ICMP black hole*).
4. Il continue d'émettre des paquets qui sont jetés en silence.

## Diagnostic

```bash
# Trouver la MTU reelle du chemin : ping avec bit Don't Fragment
ping -M do -s 1472 <destination>     # 1472 + 28 o d'en-tetes = 1500
ping -f -l 1472 <destination>        # equivalent Windows

# Descendre la taille jusqu'a ce que ca passe -> MTU = taille + 28
ip link show                          # MTU des interfaces locales
```

## Retenir

- `-s 1472` et non 1500 : `ping` compte la charge **hors** en-têtes IP (20 o) et ICMP (8 o).
- Ne jamais bloquer l'ICMP type 3 code 4 sur un firewall — c'est ce message qui fait fonctionner la découverte de MTU.

## Voir aussi

- [[encapsulation]]
- [[Types de messages ICMP]]
- [[Protocole IP (Internet Protocol)]]
- [[diagnostic-par-couche]]
