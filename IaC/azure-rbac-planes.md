---
tags: [azure, rbac, sécurité, iac]
---

# Azure RBAC — Management plane vs Data plane

Azure distingue deux niveaux d'accès aux ressources, gérés par des rôles séparés.

## Les deux plans

| Plan | Périmètre | Exemple d'opération |
|---|---|---|
| **Management plane** | Gestion des ressources ARM | Créer/supprimer un storage account, lister les access keys |
| **Data plane** | Contenu des ressources | Lire/écrire des blobs, envoyer des messages dans une queue |

## Rôles classiques et leur plan

| Rôle | Plan couvert |
|---|---|
| `Owner` | Management plane uniquement |
| `Contributor` | Management plane uniquement |
| `Reader` | Management plane (lecture) |
| `Storage Blob Data Contributor` | Data plane — blobs |
| `Storage Blob Data Reader` | Data plane — blobs (lecture) |
| `Storage Queue Data Contributor` | Data plane — queues |

**`Owner` ne donne pas accès au data plane** — il faut assigner un rôle data plane séparément.

## Cas concret : backend Terraform azurerm

- Mode **key-based** (défaut) : le backend liste les access keys via ARM → `Owner`/`Contributor` suffit
- Mode **`use_azuread_auth: true`** : le backend accède aux blobs via token AD → requiert `Storage Blob Data Contributor`

Sans ce rôle data plane, l'erreur est :
```
403 AuthorizationPermissionMismatch
This request is not authorized to perform this operation using this permission.
```

## Voir aussi

- [[atmos-azure-backend]]
- [[azurerm-credential-chain]]
- [[Azure Entra (anciennement Azure Active Directory)]]
