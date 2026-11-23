---
tags: [securite, jwt, vulnerabilites, bonnes-pratiques]
---

# JWT — Pièges et attaques classiques

La plupart des failles liées aux JWT ne cassent pas la cryptographie : elles exploitent une **validation incomplète**, ou le fait de croire ce que le jeton dit de lui-même.

## Attaques sur la validation

| Attaque | Principe | Parade |
|---|---|---|
| **`alg: none`** | Jeton sans signature, que certaines bibliothèques acceptent | Liste blanche d'algorithmes, `none` toujours refusé |
| **Confusion d'algorithme** | `alg` modifié de `RS256` en `HS256` : la clé **publique** sert alors de secret HMAC | Imposer l'algorithme côté validateur, ne jamais lire `alg` dans le jeton |
| **`jku` / `x5u` / `kid` injectés** | Le header désigne une clé contrôlée par l'attaquant | Ne récupérer les clés **que** depuis la metadata URL configurée |
| **Audience non vérifiée** | Rejeu d'un jeton destiné à une autre API | Contrôle de `aud`. Voir [[jwt-audience-issuer]] |
| **Issuer non vérifié** | Jeton d'un autre tenant, signé par les mêmes clés partagées | Contrôle de `iss` / `tid` |
| **Autorisation oubliée** | Toute application du tenant est acceptée | Exiger `roles` / `scp`. Voir [[authentification-vs-autorisation]] |

## Pièges d'exploitation

- **Pas de révocation** : un JWT volé reste valide jusqu'à `exp`. Réponses possibles :
  - durées de vie courtes ;
  - refresh tokens révocables ;
  - CAE chez Entra ID.
- **Fuite du jeton** : c'est un *bearer token*, donc quiconque le possède peut l'utiliser.
  - Ne jamais le logger. Penser à masquer `Authorization` dans les logs de passerelle et d'APM.
  - Ne jamais le passer en query string.
  - HTTPS uniquement.
- **Stockage dans un navigateur** : `localStorage` est exposé au XSS. Il vaut mieux un cookie `HttpOnly` ou un **BFF** (*Backend For Frontend*) qui garde les jetons côté serveur.
- **Décalage d'horloge** : des serveurs désynchronisés rejettent des jetons pourtant valides. Prévoir une tolérance de quelques minutes au plus (`clock-skew`).
- **Taille du jeton** : trop de groupes ou de claims peuvent faire dépasser la taille maximale des headers (proxy, IIS, Kestrel).
  - Entra ID remplace alors la liste `groups` par une référence (*groups overage*, `_claim_names`).
- **Transmettre le jeton entrant au backend** : le backend reçoit alors un jeton qui ne lui est pas destiné.
  - Supprimer le header après validation, ou obtenir un nouveau jeton (OBO, managed identity).
- **Données sensibles dans le payload** : le payload est lisible par tous. Voir [[jwt-structure]].

## Voir aussi

- [[jwt-validation-checklist]]
- [[jwt-signature-jwks]]
- [[access-token-vs-id-token]]
