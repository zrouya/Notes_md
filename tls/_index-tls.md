---
tags: [index, tls, https, securite]
---

# Index — TLS / HTTPS

## Fondamentaux

| Note | Description |
|------|-------------|
| [[https-vue-ensemble]] | Pile HTTP/TLS/TCP, 3 garanties, versions |
| [[cryptographie-hybride]] | Asymétrique (handshake) + symétrique (données) |
| [[diffie-hellman-ephemere]] | ECDHE, authentification, forward secrecy |

## Certificats

| Note | Description |
|------|-------------|
| [[certificat-x509]] | Contenu, SAN vs CN, wildcard |
| [[autorite-certification]] | CA, trust store, rôle des intermédiaires |
| [[chaine-de-certificats]] | Root/Intermediate/Leaf, validation côté client |
| [[revocation-certificat]] | CRL, OCSP, OCSP stapling |
| [[certificat-ssl]] | Placeholder |
| [[ssl]] | Placeholder |

## Handshake & connexion

| Note | Description |
|------|-------------|
| [[handshake-tls-1-2]] | Flow 2-RTT, SNI, ChangeCipherSpec, Finished |
| [[handshake-tls-1-3]] | Flow 1-RTT, certificat chiffré, algos nettoyés |
| [[connexion-https-deroule]] | DNS → TCP → TLS → HTTP, étape par étape |
| [[reprise-session-0-rtt]] | Session tickets, PSK, 0-RTT et rejeu |
| [[cout-rtt-connexion-https]] | Le budget : 3 RTT avant le premier octet utile |

## Sécurité & pratique

| Note | Description |
|------|-------------|
| [[securite-https-hsts-pinning-mitm]] | HSTS, pinning, MITM, pièges de validation |
| [[openssl]] | Inspecter handshakes, certificats et chaînes (CLI) |

## Couches en dessous

TLS repose sur TCP et n'a de sens qu'avec ce qui le précède — résolution de nom, connexion, ports.

| Note | Description |
|------|-------------|
| [[_index-reseau\|Index — Réseau]] | Le corpus réseau complet |
| [[http-versions]] | ALPN, et pourquoi HTTP/3 quitte TCP |
| [[erreurs-connexion-econnrefused]] | Situer une erreur TLS parmi les autres couches |
