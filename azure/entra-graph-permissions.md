---
tags: [azure, entra, identity, graph, oauth, iam]
---

# Entra — Permissions Microsoft Graph

Contrat d'autorisation entre une **application cliente** et une **API ressource**. Ces permissions n'appartiennent pas à Entra mais à l'API cible : Microsoft Graph n'est qu'une API parmi d'autres, n'importe quelle app peut exposer ses propres scopes et app roles (`Expose an API`).

`Group.Create` est une chaîne définie par Graph ; c'est Graph qui, à réception du token, décide qu'elle autorise `POST /groups`.

## Délégué vs Application

| | Délégué | Application (app-only) |
|---|---|---|
| Contexte | Au nom d'un utilisateur connecté | L'app pour elle-même |
| Claim du token | `scp` | `roles` |
| Droits effectifs | **Intersection** permission app ∩ droits de l'utilisateur | **Absolus, tenant-wide** |
| Objet d'octroi | `oauth2PermissionGrant` | `appRoleAssignment` |

- Délégué : une app avec `Group.ReadWrite.All` ne fera **rien de plus** que l'utilisateur derrière elle.
- Application : aucun garde-fou en amont, d'où la sensibilité de ces permissions. C'est la colonne qui compte pour un compte de service.

## Détails

- Octroi = consentement (utilisateur ou admin), matérialisé sur le **service principal**, pas sur l'app registration.
- Granularité : fine sur l'action, grossière sur le périmètre (pas de restriction de portée par défaut).

## Voir aussi

- [[entra-directory-roles]]
- [[entra-permissions-vs-roles]]
- [[entra-app-registration-vs-enterprise-app]]
- [[entra-audit-permissions-sp]]
- [[OAuth 2.0 - OpenID]]
