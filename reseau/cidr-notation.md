---
tags: [reseau, ip, subnet, azure]
---

# Notation CIDR

`/n` indique combien de **bits de tête** constituent la partie réseau d'une adresse IPv4 (32 bits au total). Le reste identifie l'hôte.

## Décomposition

```
192.168.1.10/24
11000000.10101000.00000001 . 00001010
|________ reseau 24 bits __| |_ hote _|

masque            255.255.255.0
adresse reseau    192.168.1.0      (bits d'hote a 0)
diffusion         192.168.1.255    (bits d'hote a 1)
utilisables       .1 -> .254       = 2^8 - 2 = 254 hotes
```

Les deux adresses extrêmes sont **toujours réservées** : d'où le `- 2`.

## Tailles courantes

| Préfixe | Masque | Adresses | Usage |
|---------|--------|----------|-------|
| `/32` | 255.255.255.255 | 1 | Une machine — règle de firewall, route hôte |
| `/29` | 255.255.255.248 | 8 | Petit sous-réseau de services |
| `/24` | 255.255.255.0 | 256 | Le subnet de référence |
| `/16` | 255.255.0.0 | 65 536 | Un VNet Azure, un VPC |
| `/0` | 0.0.0.0 | toutes | Route par défaut |

**Raccourci mental** : chaque bit retiré au préfixe **double** la taille du bloc.

## Plages privées (RFC 1918)

| Plage | Où on la croise |
|-------|-----------------|
| `10.0.0.0/8` | VNets Azure, VPC AWS, gros réseaux d'entreprise |
| `172.16.0.0/12` | Bridge Docker (`172.17.0.0/16`) |
| `192.168.0.0/16` | Box internet, réseaux de bureau |
| `127.0.0.0/8` | Boucle locale, ne quitte jamais la machine |
| `169.254.0.0/16` | Link-local — **symptôme d'un DHCP en échec** |

Azure réserve en plus **5 adresses par subnet** (les 4 premières et la dernière) : un `/29` n'offre que 3 IP utilisables.

## Voir aussi

- [[Masque de sous-réseau]]
- [[Sous-réseau]]
- [[NAT (Network Address Translation)]]
- [[Routage réseau]]
