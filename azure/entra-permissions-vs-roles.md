---
tags: [azure, entra, identity, iam, rbac, graph]
---

# Entra — Permissions Graph vs Rôles d'annuaire

Deux référentiels d'autorisation distincts, avec un **recouvrement partiel mais pas de doublon** : ils répondent à des questions différentes.

- **Permissions Graph** → *qu'est-ce que cette application est autorisée à demander à cette API ?*
- **Rôles d'annuaire** → *qu'est-ce que cette identité est autorisée à faire dans l'annuaire ?*

## Comparaison

| | Permissions Graph | Rôles d'annuaire |
|---|---|---|
| Défini par | L'API ressource | Entra ID |
| Porté par | Le couple (client, ressource) | Le principal |
| Octroi | Consentement → `oauth2PermissionGrant` / `appRoleAssignment` | Assignation de rôle, éventuellement PIM |
| Portée | Tenant entier (sauf exceptions) | Tenant, unité administrative, ou objet |
| Dans le token | `scp` / `roles` | Pas un scope ; `wids` pour les utilisateurs |
| Granularité | Fine sur l'action, grossière sur le périmètre | Grossière sur l'action, fine sur le périmètre |

## Origine du recouvrement

Les deux modèles viennent d'endroits opposés : les permissions Graph du monde OAuth2 / consentement (comment une app tierce obtient un accès), les rôles du monde administration AD (qui gère le tenant). Ils se croisent depuis que les service principals sont des principals de plein droit, capables de porter des rôles.

Exemple : `Group.Create` (Graph, app-only) et `Groups Administrator` (rôle) permettent tous deux de créer un groupe, mais :

- `Group.Create` — plus étroit sur l'action (créer, rien d'autre), **impossible à restreindre en périmètre**, n'existe que via Graph.
- `Groups Administrator` — beaucoup plus large sur l'action, mais **limitable à une AU**, valable tous canaux, temporisable via PIM.

## Règle de choix

- Automate faisant une opération précise et bornée via l'API → **permission Graph app-only** (moindre privilège sur l'axe action, explicite et auditable).
- Besoin de restreindre à un sous-ensemble de l'annuaire, capacité non exprimable en permission Graph, sémantique d'administration → **rôle d'annuaire**, scopé.

## Voir aussi

- [[entra-graph-permissions]]
- [[entra-directory-roles]]
- [[entra-audit-permissions-sp]]
