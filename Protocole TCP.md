---
tags: [reseau, tcp, transport, osi]
---

# Protocole TCP

**Transmission Control Protocol** — protocole de [[Couche de transport du modèle OSI|couche 4]], **orienté connexion**, fiable et ordonné.

## Les quatre garanties

| Garantie | Mécanisme |
|----------|-----------|
| Livraison | Accusés de réception (`ACK`) + retransmission sur timeout |
| Ordre | Numéros de séquence — réassemblage côté réception |
| Intégrité | Checksum sur chaque segment |
| Débit adapté | Contrôle de flux (fenêtre) + contrôle de congestion |

## Ce que ça coûte

- **1 RTT** de [[Cession TCP|handshake]] avant le moindre octet utile — voir [[cout-rtt-connexion-https]]
- En-tête de **20 octets** minimum (jusqu'à 60 avec options) — voir [[encapsulation]]
- Le *head-of-line blocking* : une perte bloque tout ce qui suit, y compris les flux indépendants multiplexés dessus (limite de HTTP/2, voir [[http-versions]])

## Identification d'une connexion

Le **quadruplet** IP source / port source / IP destination / port destination. Voir [[Ports réseaux]].

## Quand l'utiliser

Tout ce dont la perte d'un octet ruine le sens : HTTP, SSH, SQL, SMTP, réplication.

Pour l'inverse — latence prioritaire, perte tolérable — voir [[Protocole UDP]].

## Voir aussi

- [[Cession TCP]] — handshake et états
- [[ports-ephemeres-time-wait]] — TIME_WAIT et épuisement de ports
- [[erreurs-connexion-econnrefused]] — RST, refus, resets
- [[Segments (couche transport)]]
