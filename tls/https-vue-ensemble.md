---
tags: [tls, https, reseau, securite]
---

# HTTPS — Vue d'ensemble

HTTPS = **HTTP transporté dans TLS** (Transport Layer Security, successeur de SSL). Port **443** par défaut.

## La pile protocolaire

```
HTTP   (GET /index.html, en-têtes, cookies...)
 └─ TLS    (chiffre, authentifie, garantit l'intégrité)
     └─ TCP    (transport fiable)
         └─ IP
```

## Les 3 garanties de TLS

| Garantie | Mécanisme | Réponse |
|----------|-----------|---------|
| **Confidentialité** | Chiffrement symétrique (AES-GCM, ChaCha20) | Personne ne peut lire |
| **Intégrité** | AEAD / MAC | Toute altération est détectée |
| **Authenticité** | Certificats X.509 + signatures | Je parle au vrai serveur |

## Versions

- **TLS 1.2** (2008) — encore très répandu
- **TLS 1.3** (2018) — plus rapide (1-RTT), plus sûr, forward secrecy obligatoire
- ❌ Obsolètes : SSL 2/3, TLS 1.0, TLS 1.1

## Voir aussi

- [[cryptographie-hybride]]
- [[connexion-https-deroule]]
- [[handshake-tls-1-2]]
- [[handshake-tls-1-3]]
