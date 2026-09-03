---
tags: [reseau, http, quic, performance]
---

# HTTP/1.1, HTTP/2, HTTP/3

Les trois versions en service. Elles changent le **transport**, pas la sémantique : méthodes, statuts et en-têtes restent identiques.

## Comparatif

| | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Transport | TCP | TCP | **QUIC sur UDP** |
| Format | Texte | Binaire | Binaire |
| Apport | Connexions persistantes, en-tête `Host` | Multiplexage, en-têtes compressés (HPACK) | Flux réellement indépendants, migration de réseau |
| Limite | 1 requête à la fois par connexion → 6 connexions par domaine | **Head-of-line blocking TCP** : une perte fige tous les flux | Bloqué par les firewalls qui filtrent l'UDP sortant |
| Établissement | 3 RTT | 3 RTT | **0 à 1 RTT** |

## Le point clé : le head-of-line blocking

HTTP/2 multiplexe plusieurs flux dans **une seule connexion TCP**. Mais TCP garantit l'ordre : la perte d'un segment bloque la livraison de **tous** les flux, même ceux dont les données sont déjà arrivées.

QUIC résout ça en gérant l'ordre **par flux** plutôt que par connexion — d'où le passage à UDP, TCP ne pouvant pas être modifié.

## Comment la version est choisie

La négociation se joue **dans le handshake TLS**, via l'extension **ALPN**, avant la moindre requête HTTP :

- `h2` → HTTP/2
- `http/1.1` → HTTP/1.1

HTTP/3 ne peut pas être négocié en ALPN (transport différent) : le serveur l'annonce via l'en-tête `Alt-Svc`, et le client bascule sur les requêtes suivantes.

```bash
curl -v --http3 https://exemple.com     # forcer HTTP/3
curl -sI https://exemple.com | grep -i alt-svc
```

## Voir aussi

- [[cout-rtt-connexion-https]]
- [[handshake-tls-1-3]]
- [[HTTP(S)]]
- [[Requêtes HTTP]]
- [[Protocole UDP]]
