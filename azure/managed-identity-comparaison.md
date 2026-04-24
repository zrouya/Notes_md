---
tags: [azure, identity, managed-identity, iam]
---

# Managed Identity — Comparaison System vs User Assigned

| Critère | System-Assigned | User-Assigned |
|---|---|---|
| Cycle de vie | Lié à la ressource | Indépendant |
| Partage entre ressources | Non (1:1) | Oui (1:N) |
| Création | Automatique | Manuelle |
| Suppression | Automatique | Manuelle |
| Rotation des credentials | Automatique | Automatique |
| RBAC | Par ressource | Assigné une fois, réutilisable |
| Ressource Azure distincte | Non | Oui (`Microsoft.ManagedIdentity/userAssignedIdentities`) |

## Règle de choix rapide

- Ressource isolée, éphémère → **System-Assigned**
- Permissions partagées entre plusieurs ressources → **User-Assigned**
- Identité qui doit survivre à la ressource → **User-Assigned**

## Voir aussi

- [[managed-identity-system-assigned]]
- [[managed-identity-user-assigned]]
