---
tags: [securite, authentification, http]
---

# Authentification

Ensemble de mécanismes permettant à un serveur de vérifier l'identité d'un client. Les schémas HTTP historiques (Basic, Digest) sont simples mais limités en sécurité ; les protocoles modernes (OAuth 2.0, OpenID Connect) délèguent l'authentification à un fournisseur tiers.

## HTTP Basic Authentication

Protocole simple qui envoie le nom d'utilisateur et le mot de passe à chaque requête HTTP. Peu sûr, car les identifiants sont envoyés en clair (encodés en base64, facilement décodable).

## HTTP Digest Authentication

Améliore Basic Authentication en envoyant un hachage du mot de passe plutôt que le mot de passe lui-même. Reste vulnérable aux attaques par force brute et par collision si l'algorithme de hachage est faible.

## Voir aussi

- [[oauth-2-0-openid]]
