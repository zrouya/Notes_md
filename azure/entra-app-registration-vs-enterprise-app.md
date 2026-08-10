---
tags: [azure, entra, identity, iam, service-principal]
---

# Entra — App registration vs Enterprise application

Ce ne sont pas deux vues du même objet, mais **deux objets distincts** de l'annuaire.

- **`application`** (App registration) = la **définition** de l'app : `appId`, credentials, redirect URIs, permissions *demandées*, scopes et app roles *exposés*, mono/multi-tenant. Vit dans le tenant propriétaire de l'app.
- **`servicePrincipal`** (Enterprise application) = l'**instance locale** de cette app dans un tenant : l'identité concrète qui s'authentifie, à qui on accorde et assigne des choses.

Relation **1 → N** : un `application` a un service principal dans chaque tenant où l'app est utilisée. En mono-tenant on a exactement un de chaque, créés ensemble — d'où l'illusion de doublon.

## Répartition des responsabilités

| App registration (`application`) | Enterprise application (`servicePrincipal`) |
|---|---|
| Secrets et certificats | Permissions **accordées** (consentements) |
| Permissions **demandées** (`requiredResourceAccess`) | Assignation d'utilisateurs/groupes |
| API exposée : scopes et app roles définis | Rôles d'annuaire portés par l'identité |
| Redirect URIs, configuration des tokens | SSO SAML, provisioning |
| Mono / multi-tenant | Cible des politiques d'accès conditionnel |
| | Objet référencé dans les role assignments Azure RBAC |

**Conséquence pratique** : App registration montre ce que l'app *réclame*, Enterprise application ce qu'elle *détient*. Les deux divergent dès qu'un app role est accordé directement au SP — cas normal en IaC.

## SPs sans App registration

Enterprise applications contient des service principals qui n'ont **aucune** app registration dans le tenant :

- apps SaaS de la galerie ;
- apps first-party Microsoft — Microsoft Graph lui-même est un SP du tenant ;
- **managed identities**.

Une managed identity est un SP sans app registration : introuvable dans App registrations, et sans blade `API permissions`. Lui accorder une permission Graph passe obligatoirement par un `appRoleAssignment` en CLI / PowerShell / Terraform.

## Voir aussi

- [[entra-graph-permissions]]
- [[entra-audit-permissions-sp]]
- [[managed-identity-user-assigned]]
- [[managed-identity-comparaison]]
