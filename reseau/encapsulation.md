---
tags: [reseau, osi, tcp, ip]
---

# Encapsulation — chaque couche ajoute son en-tête

À l'émission, les données descendent la pile et chaque couche **préfixe son en-tête** sans jamais modifier ce qu'elle a reçu. À la réception, chaque couche retire le sien et transmet le reste au-dessus.

## Ce qui circule sur le lien

```
L7  [                    Requête HTTP 512 o                    ]
L4  [ TCP 20 o ][        Requête HTTP (inchangée)              ]
L3  [ IP 20 o ][ TCP 20 o ][   Requête HTTP (inchangée)        ]
L2  [ Eth 14 o ][ IP 20 o ][ TCP 20 o ][ HTTP ][ FCS 4 o ]
```

## Tailles des en-têtes

| Couche | En-tête | Taille min | Ce qu'il porte |
|--------|---------|-----------|----------------|
| 2 — Liaison | Ethernet | 14 o (+ 4 FCS) | MAC source → MAC de la **passerelle** |
| 3 — Réseau | IPv4 | 20 o | IP source → IP destination, TTL |
| 4 — Transport | TCP | 20 o | Ports source/destination, n° de séquence |

Minimums **sans options** : TCP et IPv4 peuvent chacun atteindre 60 o. IPv6 a un en-tête fixe de 40 o.

## Les deux réflexes à garder

- La **charge utile ne change jamais** en descendant : seuls des en-têtes s'ajoutent.
- Chaque couche ne dialogue qu'avec **son homologue en face** : ta pile TCP parle à la pile TCP distante, ta carte réseau parle au routeur du palier.

L'adresse MAC de destination est celle du **prochain saut**, pas de la destination finale — elle est réécrite à chaque saut, alors que l'IP de destination reste constante.

## Voir aussi

- [[mtu-fragmentation]]
- [[Modèle OSI]]
- [[Protocole IP (Internet Protocol)]]
- [[Segments (couche transport)]]
- [[Paquets (couche réseau)]]
