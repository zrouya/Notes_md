---
tags: [tls, cryptographie, securite]
---

# Diffie-Hellman éphémère (ECDHE) & Forward Secrecy

Mécanisme d'**échange de clé** utilisé dans le handshake pour dériver un secret partagé sans le transmettre.

## ECDHE

- **E**lliptic **C**urve **D**iffie-**H**ellman **E**phemeral
- Client et serveur échangent chacun une **part publique** (`key_share`)
- Chacun combine sa part privée + la part publique de l'autre → **même secret partagé** (le *pre-master secret*), jamais transmis sur le réseau

## Authentification

- Le serveur **signe** ses paramètres DH avec la **clé privée** de son certificat
- Prouve qu'il possède bien la clé privée correspondant à la clé publique certifiée
- ➜ c'est l'étape qui lie l'échange de clé à l'identité

## Forward Secrecy 🔑

- La clé DH est **éphémère** : jetée après la session
- Même si la clé privée du serveur est volée **plus tard**, on **ne peut pas** déchiffrer les sessions passées capturées
- L'ancien *RSA key exchange* (TLS ≤1.2) **n'offrait pas** cette propriété
- En **TLS 1.3**, le DH éphémère est **obligatoire**

## Voir aussi

- [[cryptographie-hybride]]
- [[handshake-tls-1-2]]
- [[handshake-tls-1-3]]
