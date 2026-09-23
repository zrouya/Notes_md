---
tags: [index, reseau, tcp-ip, osi]
---

# Index — Réseau

Fil conducteur recommandé : [[Modèle OSI]] → [[encapsulation]] → [[cidr-notation]] → [[Domain Name System (DNS)]] → [[Protocole TCP]] → [[cout-rtt-connexion-https]] → [[diagnostic-par-couche]].

## Modèle en couches

| Note | Description |
|------|-------------|
| [[Modèle OSI]] | Les 7 couches, modèle de référence |
| [[Modèle TCP_IP]] | La pile réellement implémentée (4 couches) |
| [[encapsulation]] | Chaque couche ajoute son en-tête — tailles réelles |
| [[Couche physique du modèle OSI]] | L1 — signaux |
| [[Couche de liaison de données du modèle OSI]] | L2 — trames, MAC |
| [[Couche réseau du modèle OSI]] | L3 — paquets, routage |
| [[Couche de transport du modèle OSI]] | L4 — segments, ports |
| [[Couche applicative du modèle OSI]] | L7 — protocoles applicatifs |
| [[Paquets (couche réseau)]] | Unité de la couche 3 |
| [[Segments (couche transport)]] | Unité de la couche 4 |
| [[Frames (couche liaison de données)]] | Unité de la couche 2 |

## Couche 2 — accès au support

| Note | Description |
|------|-------------|
| [[Ethernet]] | La norme du réseau filaire |
| [[Carrier Sense Multiple Access - Collision Detection (CSMA-CD)\|CSMA/CD]] | Détection de collision — Ethernet partagé, obsolète en commuté |
| [[Carrier Sense Multiple Access - Collision Avoidance (CSMA-CA)\|CSMA/CA]] | Évitement de collision — Wi-Fi, nœud caché, RTS/CTS |
| [[Adresses MAC]] | Adressage physique |

## Adressage

| Note | Description |
|------|-------------|
| [[Adresse IP]] | Identifie une **interface** sur un réseau |
| [[cidr-notation]] | `/24`, masques, calcul, plages privées RFC 1918 |
| [[Masque de sous-réseau]] | Séparation réseau / hôte |
| [[Sous-réseau]] | Découpage d'un réseau |
| [[IPv4]] · [[IPv6]] | Les deux familles d'adresses |
| [[Adresses MAC]] | Adresse physique de couche 2 |
| [[Adress Resolution Protocol (ARP)]] | IP → MAC sur le lien local |
| [[Adresses IP Virtuelles (VIP)]] | IP flottante, failover, load balancing |
| [[NAT (Network Address Translation)]] | Traduction privé ↔ public |

## Protocoles

| Note | Description |
|------|-------------|
| [[Protocole IP (Internet Protocol)]] | Acheminement de bout en bout, TTL, fragmentation |
| [[Protocole TCP]] | Fiable, ordonné, orienté connexion |
| [[Cession TCP]] | 3-way handshake, états de la connexion |
| [[Protocole UDP]] | Sans connexion, sans garantie |
| [[Internet Control Message Procotol (ICMP)]] | Signalisation et erreurs de couche 3 |
| [[Types de messages ICMP]] | Codes — dont `Fragmentation Needed` |
| [[Protocole IPSec]] | Chiffrement au niveau IP |
| [[Virtual Private Network (VPN)]] | Tunnel chiffré entre réseaux |

## Protocoles applicatifs

| Note | Description |
|------|-------------|
| [[Secure shell (SSH)]] | Accès distant chiffré — port 22 |
| [[File Transfert Protocol (FTP)]] | Transfert de fichiers — ports 20/21 |
| [[Remote Desktop Protocol (RDP)]] | Bureau à distance — port 3389 |

Pour HTTP, voir la rubrique dédiée plus bas.

## Étendue des réseaux

| Note | Description |
|------|-------------|
| [[Réseaux informatiques]] · [[Réseaux informatiques logiques]] | Notions de base, segmentation logique |
| [[Wide Area Network (WAN)]] | Réseau étendu |
| [[Wireless Local Network (WLAN)]] | Réseau local sans fil |

## Ports et transport

| Note | Description |
|------|-------------|
| [[Ports réseaux]] | Port = processus ; well-known vs éphémères |
| [[ports-ephemeres-time-wait]] | TIME_WAIT, épuisement de ports, SNAT Azure |
| [[mtu-fragmentation]] | MTU, tunnels, ICMP black hole |

## DNS

| Note | Description |
|------|-------------|
| [[Domain Name System (DNS)]] | Résolution hiérarchique, récursion |
| [[DNS record]] | Types A, AAAA, CNAME, MX, TXT, NS, SRV |
| [[dns-ttl-migration]] | TTL, stratégie de bascule, piège TCP/53 |
| [[Round Robin DNS]] | Répartition de charge par le DNS |
| [[dns-docker]] | Résolution entre conteneurs |

## HTTP

| Note | Description |
|------|-------------|
| [[HTTP(S)]] | Le protocole applicatif de référence |
| [[Requêtes HTTP]] | Méthodes, en-têtes, statuts |
| [[http-versions]] | HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC) |
| [[cout-rtt-connexion-https]] | 3 RTT avant le premier octet utile |
| [[connexion-https-deroule]] | DNS → TCP → TLS → HTTP, étape par étape |

Tout ce qui touche aux certificats et au handshake : voir **[[_index/tls|index TLS]]**.

## Diagnostic

| Note | Description |
|------|-------------|
| [[erreurs-connexion-econnrefused]] | **Lire un errno comme une coordonnée de couche** |
| [[diagnostic-par-couche]] | Un outil par question, dans l'ordre de la pile |
| [[mtu-fragmentation]] | Le symptôme « ça se fige sur les gros transferts » |

## Équipements

| Note | Description |
|------|-------------|
| [[Equipements réseau]] | Vue d'ensemble |
| [[Switch réseau]] | Couche 2 — lit les MAC |
| [[Routeurs réseaux]] | Couche 3 — lit les IP |
| [[Hub réseau]] | Répéteur, couche 1 (obsolète) |
| [[Passerelle réseau]] | Sortie vers les autres réseaux |
| [[Routage réseau]] | Table de routage, plus long préfixe |
| [[Firewall]] | Filtrage — `DROP` vs `REJECT` |
| [[Bastion]] | Point d'entrée d'administration |
| [[Carte d'interface réseau]] · [[Interface réseau]] · [[Network Adapter (Interface) Card]] | NIC et interfaces logiques |
| [[Application-Specific Integrated Circuitery (ASIC)]] | Puce dédiée — ce qui rend un switch rapide |

## Topologies

| Note | Description |
|------|-------------|
| [[Topologies réseaux]] | Vue d'ensemble |
| [[Topologies de réseaux câblés]] | [[Topologie réseau bus\|bus]], [[Topologie réseau en anneau\|anneau]], [[Topologie réseau en étoile\|étoile]], [[Topologie réseau en arbre\|arbre]], [[Topologie réseau mesh\|mesh]] |
| [[Topologies de réseaux sans fils]] | [[Topologie réseau ad hoc\|ad hoc]], [[Topologie réseau infrastructure\|infrastructure]] |

## Réseau Docker

| Note | Description |
|------|-------------|
| [[gestion-reseau-docker]] | Vue d'ensemble |
| [[drivers-reseau-docker]] | bridge, host, overlay, none |
| [[reseaux-docker-overlay]] | Multi-hôtes |
| [[dns-docker]] | Résolution par nom de service |

## Équipements et services réseau physiques

| Note | Description |
|------|-------------|
| [[serveur-dhcp]] | Attribution dynamique d'adresses IP |
| [[wireless-access-point-wap]] | Équivalent switch pour le sans-fil, niveau 2 OSI |
| [[wireless-range-extenders-repeteurs-wifi]] | Extension de portée Wifi |
| [[modems-modulateurs-demodulateurs]] | Passerelle LAN ↔ WAN, conversion analogique/numérique |
| [[reseaux-informatiques-physiques]] | Topologie physique — équipements, câblage |
| [[network-security-group-nsg-azure]] | Placeholder — NSG Azure |
