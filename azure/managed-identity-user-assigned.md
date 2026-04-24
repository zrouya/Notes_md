---
tags: [azure, identity, managed-identity, iam]
---

# User-Assigned Managed Identity

Identité managée créée comme **ressource Azure indépendante**, attachable à plusieurs ressources simultanément.

## Caractéristiques

- Cycle de vie **indépendant** des ressources qui l'utilisent
- **Partageable** : une identité peut être attachée à N ressources
- RBAC assigné une seule fois sur l'identité, réutilisé par toutes les ressources
- Doit être créée et supprimée manuellement

## Création (OpenTofu / Terraform)

```hcl
resource "azurerm_user_assigned_identity" "this" {
  name                = "mi-my-identity"
  resource_group_name = azurerm_resource_group.this.name
  location            = azurerm_resource_group.this.location
}

# Attacher à une ressource
resource "azurerm_linux_virtual_machine" "this" {
  # ...
  identity {
    type         = "UserAssigned"
    identity_ids = [azurerm_user_assigned_identity.this.id]
  }
}
```

## Cas d'usage

- Plusieurs ressources qui partagent les mêmes permissions (ex : N VMs → même Key Vault)
- Environnements où l'identité doit survivre à la ressource
- Scénarios multi-ressources avec un seul jeu de RBAC à maintenir

## Voir aussi

- [[managed-identity-system-assigned]]
- [[managed-identity-comparaison]]
