---
tags: [securite, authentification, autorisation, oauth, jwt]
---

# Authentification vs Autorisation

Deux questions distinctes, souvent confondues :

- **Authentification** (*AuthN*) : **qui es-tu ?** On vérifie une identité.
- **Autorisation** (*AuthZ*) : **as-tu le droit de faire ceci ?** On vérifie une permission.

## Exemple (appel d'API avec un JWT)

| Contrôle | Nature | Question posée |
|---|---|---|
| Signature, `iss`, `exp` | AuthN | Ce jeton est-il authentique, encore valide, et émis par mon fournisseur d'identité ? |
| `aud` | AuthN (au sens large) | Ce jeton m'est-il bien destiné ? |
| `roles` / `scp` | **AuthZ** | Cet appelant a-t-il la permission d'appeler cette API ? |
| Règles métier (propriétaire de la ressource…) | **AuthZ fine** | A-t-il le droit d'agir sur **cet objet-là** ? |

## Détails

- **Correspondance avec les codes HTTP** :
  - **401 Unauthorized** signifie en réalité « non authentifié » : le jeton est absent ou invalide ;
  - **403 Forbidden** signifie « authentifié, mais pas autorisé ».
- **Correspondance avec les protocoles** : **OAuth 2.0** est un protocole d'**autorisation** (déléguer un accès), **OpenID Connect** y ajoute l'**authentification** (l'ID token). Voir [[oauth-2-0-openid]].
- **Le piège** : valider uniquement la signature et l'audience ne fait que de l'authentification. Chez Entra ID, toute application du tenant peut par défaut demander un jeton pour n'importe quelle API du tenant, et ce jeton n'aura simplement **aucun rôle**. Il faut donc toujours exiger un rôle (`roles`) ou un scope (`scp`).
- **Option complémentaire côté Entra ID** : activer « Assignment required » sur l'Enterprise Application de l'API. Entra ID refuse alors **dès l'émission** de délivrer un jeton aux appelants qui n'ont pas reçu de rôle.
- **Plusieurs niveaux d'autorisation** :
  - **gros grain** à la passerelle : rôle requis pour appeler l'API ;
  - **grain fin** dans le backend : droits sur une ressource précise, utilisateur propriétaire d'une donnée…

## Voir aussi

- [[authentification]]
- [[entra-scopes-vs-app-roles]]
- [[jwt-validation-checklist]]
- [[entra-permissions-vs-roles]]
