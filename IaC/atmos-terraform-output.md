---
tags: [iac, atmos, terraform, opentofu]
---

# `!terraform.output` — Outputs cross-composants

Tag YAML Atmos pour injecter l'output Terraform d'un composant dans un autre.

## Syntaxe

```yaml
vars:
  ma_variable: !terraform.output <component-name> <output-name>
```

## Exemple

```yaml
# stacks/templates/frontwebapp/_defaults.yaml
components:
  terraform:
    webapp:
      vars:
        service_plan_id: !terraform.output appserviceplan appServicePlan_id
        resource_group_name: !terraform.output resource-group resource_group_name
```

## Comportement

1. Atmos lit le **remote state** du composant référencé dans le blob storage
2. Extrait la valeur de l'output
3. L'injecte dans le fichier `tfvars` du composant courant

## Contrainte importante

Le composant source **doit avoir été appliqué** (`terraform apply`) avant de pouvoir lire son output. Sinon : erreur de résolution du state.

**Ordre de déploiement** :
```bash
atmos terraform apply resource-group -s pfswebapp-dev
atmos terraform apply appserviceplan  -s pfswebapp-dev
atmos terraform apply webapp          -s pfswebapp-dev
```

## Différence avec `atmos.Component`

`!terraform.output` est résolu à l'injection des tfvars, **pas** au rendu Go template. Il n'est donc **pas utilisable** dans `{{ .vars.X }}` ou dans `import_resource_id` calculé par Go template.

```yaml
# ❌ Ne fonctionne pas — resource_group_name via !terraform.output
# n'est pas disponible comme .vars.resource_group_name dans les Go templates
import_resource_id: "/.../.../{{ .vars.resource_group_name }}/..."
```

## Voir aussi

- [[atmos-component-function]]
- [[atmos-go-templates]]
- [[opentofu-import]]
