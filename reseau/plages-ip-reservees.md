---
tags: [reseau, ip, adressage, ipv4, ipv6, rfc]
---

# Plages IP réservées

Blocs d'adresses à usage spécial, définis par l'IETF et recensés par l'IANA (registre RFC 6890). Ils ne sont **jamais attribués comme IP publiques**.

## IPv4

| Plage | Usage | RFC |
|-------|-------|-----|
| `0.0.0.0/8` | « Ce réseau » — `0.0.0.0` = toutes interfaces (bind) ou route par défaut | 1122 |
| `10.0.0.0/8` | **Privé** | 1918 |
| `100.64.0.0/10` | **CGNAT** — NAT côté opérateur (box 4G/fibre partagée) | 6598 |
| `127.0.0.0/8` | **Loopback** — ne quitte jamais la machine | 1122 |
| `169.254.0.0/16` | **Link-local** — auto-attribuée si DHCP en échec ; métadonnées cloud `169.254.169.254` | 3927 |
| `172.16.0.0/12` | **Privé** (`172.16` → `172.31`) — bridge Docker `172.17.0.0/16` | 1918 |
| `192.0.2.0/24` · `198.51.100.0/24` · `203.0.113.0/24` | **Documentation** (TEST-NET-1/2/3) — pour les exemples | 5737 |
| `192.168.0.0/16` | **Privé** | 1918 |
| `198.18.0.0/15` | Tests de performance (benchmark) | 2544 |
| `224.0.0.0/4` | **Multicast** | 5771 |
| `240.0.0.0/4` | Réservé (classe E, inutilisable) | 1112 |
| `255.255.255.255/32` | Broadcast limité | 919 |

**Piège** : `172.16.0.0/12` s'arrête à `172.31.255.255` — `172.32.x.x` est **publique**.

## IPv6

| Plage | Usage |
|-------|-------|
| `::/128` | Adresse non spécifiée (équivalent `0.0.0.0`) |
| `::1/128` | Loopback |
| `fe80::/10` | Link-local — présente sur **toute** interface IPv6 |
| `fc00::/7` (en pratique `fd00::/8`) | **ULA** — équivalent des plages privées |
| `ff00::/8` | Multicast (IPv6 n'a pas de broadcast) |
| `2001:db8::/32` | Documentation |
| `::ffff:0:0/96` | IPv4 mappée (`::ffff:192.168.1.10`) |
| `64:ff9b::/96` | NAT64 |

## Spécificités cloud

- `169.254.169.254` : endpoint de **métadonnées d'instance** (Azure IMDS, AWS, GCP).
- Azure : `168.63.129.16` (IP publique virtuelle de la plateforme : DNS Azure, health probes) et 5 IP réservées par subnet — voir [[cidr-notation]].

## Voir aussi

- [[ip-publique-vs-privee]]
- [[cidr-notation]]
- [[IPv4]] · [[IPv6]]
