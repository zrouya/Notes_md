---
tags: [tls, handshake, performance, securite]
---

# Reprise de session & 0-RTT

Mécanismes pour éviter de refaire un handshake complet lors d'une reconnexion.

## Pourquoi

Un handshake complet coûte un (TLS 1.3) ou deux (TLS 1.2) aller-retours + crypto asymétrique. Sur des connexions répétées vers le même serveur, on peut réutiliser un secret déjà négocié.

## Mécanismes

| Mécanisme | Version | Principe |
|-----------|---------|----------|
| **Session ID** | TLS 1.2 | Le serveur garde l'état de session en mémoire |
| **Session Ticket** | TLS 1.2/1.3 | Le serveur chiffre l'état et le confie au client (stateless) |
| **PSK** (Pre-Shared Key) | TLS 1.3 | Le ticket devient une clé pré-partagée pour la reprise |

## 0-RTT (TLS 1.3)

- Sur une **reconnexion**, le client envoie de la **donnée applicative dès le 1er paquet**, sans attendre la fin du handshake
- ⚡ Latence quasi nulle
- ⚠️ **Compromis de sécurité** : les données 0-RTT sont **vulnérables au rejeu** (replay)
- ➜ À réserver aux requêtes **idempotentes** (GET), jamais à un POST qui modifie l'état

## Voir aussi

- [[handshake-tls-1-3]]
- [[connexion-https-deroule]]
- [[diffie-hellman-ephemere]]
