---
tags: [iac, atmos, yaml]
---

# Atmos — Composition de stacks

Les stacks sont composées par imports en trois couches.

## Architecture en couches

```
stacks/
├── mixins/              # Concerns transverses (backend, provider)
├── catalog/             # Abstractions réutilisables par type de ressource
│   ├── _defaults.yaml        # Imports mixins + convention de nommage
│   ├── resourceGroup/
│   │   └── _defaults.yaml    # Formule naming + import_resource_id
│   └── webapp/
│       ├── _defaults.yaml    # Formule naming + import_resource_id
│       └── dev.yaml          # Abstract component dev
└── templates/
    ├── project-config/       # Config projet (agnostique environnement)
    └── frontwebapp/
        ├── _defaults.yaml    # Vars template + composants de base
        └── dev.yaml          # Stack entry point
```

## Fichier entry point (ex: dev.yaml)

```yaml
import:
  - "./_defaults.yaml"
  - "./project-config/mon-projet_project-config.yaml"
  - catalog/webapp/dev
  - catalog/resourceGroup/_defaults

vars:
  environment: dev
  subscription_id: xxxxxx
```

## Catalog — composant abstrait

```yaml
# catalog/webapp/_defaults.yaml
import:
  - catalog/_defaults

components:
  terraform:
    webapp/defaults:
      metadata:
        component: webapp
        type: abstract
      vars:
        resource_type: app
        web_app_name: "ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-app-{{ .vars.resource_identifier }}"
```

## Héritage de composants

```yaml
components:
  terraform:
    webapp:
      metadata:
        component: webapp
        inherits:
          - webapp/defaults   # hérite du catalog abstrait
```

## Voir aussi

- [[atmos-overview]]
- [[atmos-go-templates]]
- [[atmos-component-function]]
