---
tags: [iac, atmos, go-templates]
---

# `atmos.Component` — Accès cross-composant

Fonction Go template Atmos pour lire les vars résolues d'un autre composant dans le même stack.

## Syntaxe

```yaml
{{ (atmos.Component "<component-name>" .atmos_stack).<section>.<variable> }}
```

## Exemple — récupérer le nom du resource group depuis webapp

```yaml
# catalog/webapp/_defaults.yaml
components:
  terraform:
    webapp/defaults:
      vars:
        resource_group_name: '{{ (atmos.Component "resource-group" .atmos_stack).vars.resource_group_name }}'
        import_resource_id: '/subscriptions/{{ .vars.subscription_id }}/resourceGroups/{{ (atmos.Component "resource-group" .atmos_stack).vars.resource_group_name }}/providers/Microsoft.Web/sites/ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-app-{{ .vars.resource_identifier }}'
```

## Avantages vs `!terraform.output`

| | `atmos.Component` | `!terraform.output` |
|---|---|---|
| Nécessite un déploiement préalable | ❌ Non | ✅ Oui |
| Accède aux vars Atmos | ✅ Oui | ❌ Non |
| Accède aux outputs Terraform | ❌ Non | ✅ Oui |
| Disponible au rendu YAML | ✅ Oui | ❌ Non (injection tfvars) |

## Cas d'usage

- Référencer le nom d'un RG calculé par un autre composant
- Construire un `import_resource_id` qui dépend d'un autre composant
- Éviter de redéclarer une valeur déjà calculée ailleurs

## Voir aussi

- [[atmos-go-templates]]
- [[atmos-terraform-output]]
