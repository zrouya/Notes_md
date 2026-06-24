---
tags: [tls, certificat, x509, securite]
---

# Certificat X.509

Document **signé numériquement** qui lie une **identité** (nom de domaine) à une **clé publique**.

## Contenu principal

| Champ | Rôle |
|-------|------|
| **Subject** | À qui appartient le certificat |
| **SAN** (Subject Alternative Names) | Liste des domaines couverts : `example.com`, `*.example.com` |
| **Public Key** | Clé publique du serveur |
| **Issuer** | Qui a émis/signé (la CA) |
| **Validity** | `Not Before` / `Not After` |
| **Signature** | Signature de l'émetteur sur tout le reste |

## CN vs SAN ⚠️

- Le **CN** (`Common Name`) est **ignoré** par les navigateurs modernes
- Seul le **SAN** est utilisé pour valider le domaine
- Un certificat sans SAN couvrant le domaine demandé → erreur

## Wildcard

- `*.example.com` couvre **un seul niveau** : `a.example.com` ✅
- Ne couvre **pas** : `a.b.example.com` ❌, ni `example.com` nu ❌

## Voir aussi

- [[autorite-certification]]
- [[chaine-de-certificats]]
- [[openssl]]
