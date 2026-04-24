---
tags: [iac, opentofu, terraform, azure]
---

# OpenTofu — Import de ressources existantes

Permet d'importer une ressource Azure existante dans le state Terraform sans la recréer.

## Pattern dans les modules

```hcl
# import.tf
import {
  for_each = var.provisioning_type == "provisioning_withImport" ? { key = var.import_resource_id } : {}
  to       = azurerm_windows_web_app.webapp[0]
  id       = each.value
}
```

## Variables associées

```hcl
variable "provisioning_type" {
  type    = string
  default = "provisioning"
  # Valeurs : "provisioning" | "provisioning_withImport" | "data_lookup"
}

variable "import_resource_id" {
  type    = string
  default = ""
  # ARM ID complet de la ressource Azure
}
```

## Modes de lifecycle

| Mode | `provisioning_type` | `import_resource_id` | Comportement |
|---|---|---|---|
| Créer | `provisioning` | `""` | Crée la ressource |
| Importer | `provisioning_withImport` | ARM ID | Importe dans le state |
| Lookup | `data_lookup` | `""` | Lit sans gérer le state |

## Format des ARM IDs Azure

```
# Resource Group
/subscriptions/<sub_id>/resourceGroups/<rg_name>

# Web App
/subscriptions/<sub_id>/resourceGroups/<rg_name>/providers/Microsoft.Web/sites/<app_name>

# App Service Plan
/subscriptions/<sub_id>/resourceGroups/<rg_name>/providers/Microsoft.Web/serverfarms/<plan_name>
```

## Intégration Atmos

```yaml
# Dans le catalog (catalog/webapp/_defaults.yaml)
vars:
  import_resource_id: '/subscriptions/{{ .vars.subscription_id }}/resourceGroups/{{ (atmos.Component "resource-group" .atmos_stack).vars.resource_group_name }}/providers/Microsoft.Web/sites/ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-app-{{ .vars.resource_identifier }}'
```

## Voir aussi

- [[atmos-component-function]]
- [[atmos-terraform-output]]
