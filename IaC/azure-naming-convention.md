---
tags: [azure, iac, naming, convention]
---

# Convention de nommage Azure (Entoria)

Pattern standard pour les ressources Azure déployées via Atmos/OpenTofu.

## Formule

```
ent-az-we-<environment>-pfs-<domain>-<resource_type>-<resource_identifier>
```

## Segments

| Segment | Description | Exemples |
|---|---|---|
| `ent` | Entreprise | fixe |
| `az` | Cloud | fixe |
| `we` | Région (West Europe) | fixe |
| `environment` | Environnement | `dev`, `rec`, `prod` |
| `pfs` | Produit / BU | fixe |
| `domain` | Domaine applicatif | `apr`, `web`, `gst` |
| `resource_type` | Type de ressource | `rg`, `app`, `plan`, `func` |
| `resource_identifier` | Index | `001`, `006` |

## Exemples concrets

```
ent-az-we-dev-pfs-web-rg-006       # Resource Group
ent-az-we-dev-pfs-apr-app-001      # Web App
ent-az-we-dev-pfs-web-plan-001     # App Service Plan
ent-az-we-dev-pfs-gst-func-001     # Function App
```

## Implémentation Atmos (dans les catalogs)

```yaml
# catalog/webapp/_defaults.yaml
vars:
  resource_type: app
  web_app_name: "ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-app-{{ .vars.resource_identifier }}"

# catalog/resourceGroup/_defaults.yaml
vars:
  resource_type: rg
  resource_group_name: "ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-rg-{{ .vars.resource_identifier }}"
```

## Vars à fournir par le fichier de stack ou project-config

```yaml
vars:
  environment: dev
  domain: apr
  resource_identifier: "001"
```

## Voir aussi

- [[atmos-stack-composition]]
- [[atmos-go-templates]]
- [[opentofu-import]]
