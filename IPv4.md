---
tags: [reseau, ip, adressage]
---

# IPv4

Adresse sur **32 bits**, écrite en quatre octets décimaux séparés par des points : `192.168.1.10`.

Une adresse IPv4 ne désigne pas une machine mais **une interface sur un réseau** — une machine à deux cartes a deux adresses.

## Structure

Les 32 bits se scindent en deux parties, dont la frontière est fixée par le [[Masque de sous-réseau|masque]] :

- un préfixe **réseau**, identique pour toutes les machines du segment
- un suffixe **hôte**, propre à chaque machine

Voir [[cidr-notation]] pour le calcul et les tailles de blocs.

## En-tête

**20 octets** minimum (jusqu'à 60 avec options). Champs notables :

| Champ | Rôle |
|-------|------|
| Source / Destination | Les deux adresses, constantes de bout en bout |
| **TTL** | Décrémenté à chaque saut ; à 0 le paquet est détruit |
| Protocole | 6 = TCP, 17 = UDP, 1 = ICMP |
| Flags / Offset | Fragmentation — voir [[mtu-fragmentation]] |

Le TTL à 0 déclenche un ICMP `Time Exceeded` vers l'émetteur : c'est exactement le mécanisme qu'exploite `traceroute`.

## Épuisement

Les ~4,3 milliards d'adresses sont épuisées depuis 2011. Deux réponses coexistent : le [[NAT (Network Address Translation)|NAT]] avec les plages privées RFC 1918, et [[IPv6]].

## Voir aussi

- [[Adresse IP]] · [[cidr-notation]]
- [[Protocole IP (Internet Protocol)]]
- [[Routage réseau]]
