---
tags: [iac, atmos, azure, terraform, backend]
---

# Atmos — Backend Azure Blob Storage

Configuration du backend azurerm pour stocker l'état OpenTofu dans Azure Blob Storage.

## Configuration minimale (key-based — fonctionne avec Owner/Contributor)

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
```

Le backend liste les access keys du storage via ARM (management plane) → requiert `Owner` ou `Contributor` sur le storage account.

## Configuration Azure AD (use_azuread_auth)

```yaml
      use_azuread_auth: true   # accès data plane via token AD
```

- Évite l'usage des access keys (meilleure sécurité)
- **Prérequis strict** : rôle `Storage Blob Data Contributor` sur le storage account
- `Owner` ou `Contributor` **ne suffisent pas** — ce sont des rôles management plane uniquement
- Sans ce rôle data plane → `403 AuthorizationPermissionMismatch`

Voir [[azure-rbac-planes]] pour la distinction management plane / data plane.

## Choisir entre les deux modes

| Critère | Key-based (défaut) | `use_azuread_auth: true` |
|---|---|---|
| Rôle requis | `Owner` / `Contributor` | `Storage Blob Data Contributor` |
| Sécurité | Accès via clé partagée | Token AD éphémère |
| Compatibilité | Universelle | Nécessite rôle data plane |

## `auto_generate_backend_file: true`

Défini dans `atmos.yaml`. Génère le fichier `backend.tf.json` localement avant `tofu init`.  
**Ne crée pas** les ressources Azure (storage account, container).

## Provisionnement automatique du container

```yaml
terraform:
  provision:
    backend:
      enabled: true
```

Atmos peut créer le container blob s'il n'existe pas (introduit en v1.204.0).  
Initialement conçu pour S3 — le support Azure est moins documenté, à valider par un test.

## Voir aussi

- [[atmos-azure-auth]]
- [[azure-rbac-planes]]
- [[azurerm-credential-chain]]
- [[atmos-overview]]
