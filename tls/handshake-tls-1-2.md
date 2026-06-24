---
tags: [tls, handshake, securite]
---

# Handshake TLS 1.2

Négociation en **2 aller-retours** (après le 3-way handshake TCP), avant tout échange de données.

## Déroulé

```
CLIENT                                    SERVEUR
  │ ──────────── ClientHello ──────────────► │
  │   versions, cipher suites,               │
  │   Client Random, SNI, ALPN               │
  │ ◄─────────── ServerHello ─────────────── │
  │   version + cipher choisis, Server Random│
  │ ◄─────────── Certificate ─────────────── │
  │   chaîne feuille + intermédiaires        │
  │ ◄─────────── ServerKeyExchange ───────── │
  │   params DH éphémère, SIGNÉS             │
  │ ◄─────────── ServerHelloDone ─────────── │
  │   [client valide le certificat]          │
  │ ──────────── ClientKeyExchange ────────► │
  │   sa part DH éphémère                    │
  │ ──────────── ChangeCipherSpec ─────────► │
  │ ──────────── Finished (chiffré) ───────► │
  │ ◄─────────── ChangeCipherSpec ────────── │
  │ ◄─────────── Finished (chiffré) ──────── │
  │ ═══════ données applicatives chiffrées ═ │
```

## Éléments clés

- **SNI** (Server Name Indication) : domaine demandé, **en clair** dans le ClientHello. Indispensable pour l'hébergement mutualisé (le serveur choisit quel certificat présenter).
- **Client/Server Random** : aléas qui rendent chaque session unique (anti-rejeu).
- **ECDHE** : voir [[diffie-hellman-ephemere]]. Le serveur **signe** ses params → authentification.
- **ChangeCipherSpec** : "à partir de maintenant je chiffre".
- **Finished** : hash chiffré de tout le handshake → détecte toute altération de la négociation.

## Voir aussi

- [[handshake-tls-1-3]]
- [[diffie-hellman-ephemere]]
- [[connexion-https-deroule]]
