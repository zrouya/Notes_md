---
tags: [reseau, ip, adressage]
---

# IPv6

Adresse sur **128 bits**, notée en hexadécimal par groupes de 16 bits : `2001:0db8:0000:0000:0000:0000:0000:0001`.

## Écriture abrégée

```
2001:0db8:0000:0000:0000:0000:0000:0001
2001:db8::1        <- zeros de tete supprimes + :: pour une suite de zeros
```

`::` ne peut apparaître **qu'une seule fois** dans une adresse (sinon l'expansion serait ambiguë).

Dans une URL, l'adresse se met entre crochets pour ne pas confondre avec le port : `http://[2001:db8::1]:8080/`.

## Différences avec [[IPv4]]

| | IPv4 | IPv6 |
|---|---|---|
| Taille | 32 bits | 128 bits |
| En-tête | 20–60 o, variable | **40 o, fixe** |
| Fragmentation | Par les routeurs | Par l'émetteur uniquement |
| [[NAT (Network Address Translation)\|NAT]] | Omniprésent | **Aucun** |
| Config auto | DHCP | SLAAC (autoconfiguration) |
| Broadcast | Oui | Remplacé par le multicast |

Le préfixe standard attribué à un site est un `/64`.

## Le point de vigilance

**Pas de NAT** : chaque machine a une adresse publique routable. La sécurité repose donc **entièrement sur le firewall** — l'effet de bord protecteur du NAT disparaît.

## Dual stack

Les deux piles coexistent en pratique. Un client moderne applique *Happy Eyeballs* : il tente IPv6 et retombe sur IPv4 après un court délai.

Conséquence en exploitation : **des pannes qui n'affectent qu'une des deux familles** et se manifestent par des lenteurs intermittentes. Forcer une famille pour tester :

```bash
curl -4 https://exemple.com
curl -6 https://exemple.com
dig AAAA exemple.com
```

## Voir aussi

- [[Adresse IP]] · [[cidr-notation]]
- [[Protocole IP (Internet Protocol)]]
