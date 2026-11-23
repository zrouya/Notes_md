---
tags: [securite, jwt, token, claims]
---

# JWT — Les claims

Un **claim** est une affirmation portée par le jeton, sous forme de paire clé/valeur dans le payload : « je suis émis par X », « je suis destiné à Y », « le porteur a le rôle Z ».

## Claims standards (RFC 7519)

| Claim | Nom | Question à laquelle il répond |
|---|---|---|
| `iss` | Issuer | **Qui** a émis le jeton ? |
| `aud` | Audience | **À qui** le jeton est-il destiné ? |
| `sub` | Subject | **De qui** parle le jeton (utilisateur ou application) ? |
| `exp` | Expiration | Jusqu'à **quand** le jeton est-il valide ? |
| `nbf` | Not Before | À partir de **quand** est-il valide ? |
| `iat` | Issued At | **Quand** a-t-il été émis ? |
| `jti` | JWT ID | Identifiant unique du jeton (sert à détecter un rejeu) |

Les dates sont exprimées en **secondes depuis l'epoch Unix**. Exemple : `1764000000` correspond au 24/11/2025.

## Claims liés à l'autorisation (OAuth 2.0)

| Claim | Contenu | Usage |
|---|---|---|
| `scp` / `scope` | Permissions **déléguées** | Une application agit **au nom d'un utilisateur** |
| `roles` | Rôles applicatifs | Application seule, ou rôle attribué à un utilisateur |
| `azp` / `client_id` | Application cliente | **Quelle application** a demandé le jeton |

Le détail des différences entre `scp` et `roles` est dans [[entra-scopes-vs-app-roles]].

## Détails

- **Claims enregistrés** (*registered*) : définis par la RFC, à sémantique fixe (`iss`, `aud`, `exp`…).
- **Claims publics et privés** : ajoutés librement par chaque émetteur. Entra ID ajoute par exemple `tid`, `oid` et `appid`. Voir [[entra-access-token-claims]].
- **Valeur simple ou liste** : `aud` peut être une chaîne ou un tableau, `roles` est toujours un tableau.
- **Deux claims à ne pas confondre** :
  - `sub` identifie le sujet **pour une application donnée**. Chez Entra ID, sa valeur diffère d'une application à l'autre pour le même utilisateur.
  - `oid` (Entra ID) identifie l'objet **de manière stable dans tout le tenant**. C'est lui qu'il faut utiliser comme clé d'identité.
- **Un claim n'a de valeur que si la signature a été vérifiée** : sans cette vérification, n'importe qui peut forger un payload. Voir [[jwt-validation-checklist]].

## Voir aussi

- [[jwt-structure]]
- [[jwt-audience-issuer]]
- [[entra-access-token-claims]]
