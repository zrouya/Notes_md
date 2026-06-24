---
tags: [tls, cryptographie, securite]
---

# Cryptographie hybride (TLS)

TLS combine deux familles de cryptographie, chacune pour ce qu'elle fait de mieux.

## Asymétrique (clé publique / clé privée)

- Algos : RSA, ECDSA (signature), ECDHE (échange de clé)
- **Lente**, mais permet à deux inconnus de s'authentifier et d'échanger un secret **sans canal sécurisé préalable**
- Utilisée **uniquement au début**, pendant le handshake

## Symétrique (une seule clé partagée)

- Algos : AES-GCM, ChaCha20-Poly1305
- **Très rapide**
- Chiffre **toutes les données applicatives** une fois le handshake terminé

## Le principe

> On utilise l'asymétrique (coûteux) juste le temps de négocier une **clé de session symétrique**, puis on bascule sur le symétrique (rapide) pour le reste de la connexion.

C'est le *chiffrement hybride*. La clé de session est dérivée via [[diffie-hellman-ephemere]] sans jamais transiter sur le réseau.

## Voir aussi

- [[diffie-hellman-ephemere]]
- [[handshake-tls-1-2]]
- [[https-vue-ensemble]]
