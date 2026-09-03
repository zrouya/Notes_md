---
tags: [reseau, udp, transport, osi]
---

# Protocole UDP

**User Datagram Protocol** — protocole de [[Couche de transport du modèle OSI|couche 4]] **sans connexion**. On émet, c'est tout.

## Ce qu'il ne fait pas

- Pas de handshake → **aucun coût d'entrée**
- Pas d'accusé de réception → perte **silencieuse**
- Pas de numéro de séquence → aucun ordre garanti
- Pas de contrôle de congestion → émet à la vitesse voulue

En-tête de **8 octets** seulement (contre 20 pour TCP) : ports source/destination, longueur, checksum.

## TCP ou UDP

| | [[Protocole TCP\|TCP]] | UDP |
|---|---|---|
| Connexion | Handshake 3 temps | Aucune |
| Fiabilité | Retransmission | Perte silencieuse |
| Ordre | Garanti | Aucun |
| Coût d'entrée | 1 RTT | Nul |
| Pour quoi | HTTP, SSH, SQL | DNS, DHCP, NTP, VoIP, jeux, **QUIC** |

Le critère : **la latence prime-t-elle sur l'exhaustivité ?** En VoIP, retransmettre un paquet audio arrivé trop tard n'a aucun intérêt.

## Le cas QUIC

QUIC (transport de HTTP/3) est bâti sur UDP, mais **réimplémente la fiabilité au-dessus** — par flux plutôt que par connexion. Ce n'est pas « UDP parce que c'est plus simple », c'est UDP parce que TCP ne peut plus être modifié dans les équipements existants. Voir [[http-versions]].

## Piège d'exploitation

Un firewall qui bloque l'UDP sortant casse HTTP/3 **silencieusement** (repli sur HTTP/2) et peut casser le DNS. Voir [[dns-ttl-migration]] pour le cas `tcp/53`.

## Voir aussi

- [[Ports réseaux]]
- [[Domain Name System (DNS)]]
- [[Segments (couche transport)]]
