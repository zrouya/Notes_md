---
tags: [iac, atmos, opentofu, terraform]
---

# Atmos

Orchestrateur de stacks IaC. Enveloppe OpenTofu/Terraform avec un système de composition YAML.

## Commandes essentielles

```bash
# Lister les stacks
atmos list stacks

# Plan / Apply / Deploy d'un composant
atmos terraform plan   <component> -s <stack>
atmos terraform apply  <component> -s <stack>
atmos terraform deploy <component> -s <stack>

# Valider la configuration des stacks
atmos validate stacks
```

## Configuration — atmos.yaml

```yaml
components:
  terraform:
    command: tofu          # binaire à utiliser
    base_path: "./modules"
    auto_generate_backend_file: true

stacks:
  base_path: "./stacks"
  included_paths:
    - "templates/**/*.yaml"
  excluded_paths:
    - "**/_defaults.yaml"
    - "**/project-config/**"
  name_template: "{{.vars.template}}-{{.vars.environment}}"

templates:
  settings:
    enabled: true
    sprig:
      enabled: true   # fonctions Sprig (printf, etc.)
```

## Variables d'environnement Azure

```
ARM_SUBSCRIPTION_ID
ARM_TENANT_ID
ARM_CLIENT_ID
ARM_CLIENT_SECRET
```

## Voir aussi

- [[atmos-stack-composition]]
- [[atmos-go-templates]]
- [[atmos-stack-discovery]]
