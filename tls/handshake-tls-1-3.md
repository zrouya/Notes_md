---
tags: [tls, handshake, securite]
---

# Handshake TLS 1.3

Version nettoyée et accélérée : **1 seul aller-retour** (1-RTT) au lieu de 2.

## L'astuce

Le client **devine** que le serveur supportera ECDHE et envoie sa part de clé (`key_share`) **dès le ClientHello**.

```
CLIENT                                    SERVEUR
  │ ──────────── ClientHello ──────────────► │
  │   cipher suites, key_share (client),     │
  │   SNI, ALPN                              │
  │ ◄─────────── ServerHello ─────────────── │
  │   key_share (serveur)                    │
  │   ─── à partir d'ici tout est chiffré ── │
  │   {Certificate}                          │
  │   {CertificateVerify}  (signature)       │
  │   {Finished}                             │
  │ ──────────── {Finished} ───────────────► │
  │ ═══════ données applicatives chiffrées ═ │
```

## Différences vs TLS 1.2

- **1-RTT** au lieu de 2 → connexion plus rapide
- Le **certificat est chiffré** (meilleure confidentialité)
- Algos faibles **supprimés** : RSA key exchange, CBC, RC4, compression
- **Forward secrecy obligatoire** (ECDHE imposé)
- Mode **0-RTT** possible en reprise de session → voir [[reprise-session-0-rtt]]

## Voir aussi

- [[handshake-tls-1-2]]
- [[reprise-session-0-rtt]]
- [[diffie-hellman-ephemere]]
