---
tags: [iac, atmos, azure, terraform, backend]
---

# Atmos — Backend Azure Blob Storage

Configuration du backend azurerm pour stocker l'état OpenTofu dans Azure Blob Storage.

## Configuration de base

```yaml
# stacks/mixins/backend.yaml
terraform:
  backend_type: azurerm
  backend:
    azurerm:
      resource_group_name: "mon-rg"
      storage_account_name: "monstorageaccount"
      container_name: "{{ .vars.project_identifier }}"
      key: "{{ .component }}/terraform.tfstate"
      use_azuread_auth: true   # recommandé : Azure AD RBAC au lieu des access keys
```

## `use_azuread_auth: true`

- Utilise un token Azure AD pour accéder au blob (via `AzureCLICredential`)
- Évite de lister les storage account access keys (méthode legacy)
- Équivalent à la variable d'env `ARM_USE_AZUREAD=true` — les deux sont interchangeables, inutile de définir les deux
- **Prérequis** : le Service Principal doit avoir le rôle `Storage Blob Data Contributor` sur le storage account (pas seulement `Contributor`)

## `auto_generate_backend_file: true`

Défini dans `atmos.yaml`. Génère le fichier `backend.tf.json` localement avant `tofu init`.  
**Ne crée pas** les ressources Azure (storage account, container).

## Provisionnement automatique du container

```yaml
# backend.yaml
terraform:
  provision:
    backend:
      enabled: true
```

Atmos peut créer le container blob s'il n'existe pas (introduit en v1.204.0).  
Initialement conçu pour S3 — le support Azure est moins documenté, à valider par un test.

## Rôles RBAC nécessaires sur le storage account

| Rôle | Utilité |
|---|---|
| `Storage Blob Data Contributor` | Lecture/écriture du state blob (requis avec `use_azuread_auth`) |
| `Contributor` (resource group) | Gestion des ressources ARM |

## Voir aussi

- [[atmos-azure-auth]]
- [[atmos-overview]]
