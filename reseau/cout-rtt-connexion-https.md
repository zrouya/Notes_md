---
tags: [reseau, http, tls, performance]
---

# Le coût en RTT d'une connexion HTTPS

Avant le **premier octet utile**, une requête HTTPS « à froid » consomme trois allers-retours.

## Le budget

| Phase | Coût | Ce qui se joue |
|-------|------|----------------|
| Résolution DNS | ~20 ms (ou 0 si en cache) | Nom → adresse IP |
| Handshake TCP | **1 RTT** | SYN / SYN-ACK / ACK |
| Handshake TLS 1.3 | **1 RTT** | ClientHello (SNI + ALPN) / ServerHello+cert / Finished |
| Requête HTTP | **1 RTT** | GET → 200 OK |

Sur un lien à 40 ms de latence : **120 ms perdus d'avance**, avant même que le serveur ne commence à travailler.

## Conséquence pratique

> Réutiliser une connexion change davantage les temps de réponse que d'optimiser le serveur.

Sur un appel suivant, seule la dernière phase subsiste :

| Situation | Coût |
|-----------|------|
| Connexion à froid | 3 RTT |
| Keep-alive / pool de connexions | **1 RTT** |
| Reprise de session TLS (0-RTT) | 1 RTT, sans re-handshake |
| HTTP/3 (QUIC), reprise | 0-RTT |

## Ce qui réduit le budget

- **Keep-alive et pool de connexions** — le levier n°1, voir [[ports-ephemeres-time-wait]]
- **TLS 1.3** — 1 RTT au lieu de 2 en TLS 1.2, voir [[handshake-tls-1-3]]
- **[[reprise-session-0-rtt]]** — session tickets, PSK
- **HTTP/3** — fusionne établissement de connexion et TLS, voir [[http-versions]]
- **Cache DNS** — un TTL trop court recrée une résolution à chaque appel, voir [[dns-ttl-migration]]

## Voir aussi

- [[connexion-https-deroule]]
- [[Requêtes HTTP]]
