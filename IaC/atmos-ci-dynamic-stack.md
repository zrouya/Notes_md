---
tags: [iac, atmos, gitlab-ci, pattern]
---

# Atmos — Assemblage dynamique de stack en CI

Pattern permettant à un projet applicatif de surcharger une stack Atmos sans connaître la structure de fichiers Atmos.

## Principe

Le projet fournit un fichier YAML d'overrides (sans `import:` ni `environment`).  
Le job CI génère le fichier stack complet en injectant l'import du blueprint et l'environnement.

## Fichier fourni par le projet

```yaml
# atmos-config.yaml — dans le repo applicatif
vars:
  template: myapp
  resource_identifier: "001"
  resource_group_name: "ent-az-we-cdh-rg-001"

components:
  terraform:
    sqlAppAccess/ca1-to-db1:
      metadata:
        inherits: [sqlAppAccess/defaults]
      vars:
        app_resource_identifier: "001"
        sql_resource_identifier: "001"
```

## Variables CI du projet

| Variable | Exemple | Rôle |
|---|---|---|
| `ATMOS_TEMPLATE` | `myapp` | Nom du template (= stack sans l'env) |
| `ATMOS_BLUEPRINT` | `containerapp-sql-access` | Blueprint Atmos à utiliser |

## Job CI — assemblage + déploiement

```bash
STACK_NAME="${ATMOS_TEMPLATE}-${CI_ENVIRONMENT_NAME}"

# En-tête injecté par le CI
cat > /tmp/atmos-header.yaml << EOF
import:
  - blueprints/${ATMOS_BLUEPRINT}
vars:
  environment: ${CI_ENVIRONMENT_NAME}
EOF

# Deep merge : header sert de base, project-config surcharge
yq eval-all '. as $item ireduce ({}; . *+ $item)' \
  /tmp/atmos-header.yaml \
  atmos-config.yaml \
  > stacks/templates/${ATMOS_TEMPLATE}/${STACK_NAME}.yaml

# Découverte des composants déployables (non abstraits)
COMPONENTS=$(atmos describe stacks --stack "${STACK_NAME}" \
  --query '.[].components.terraform | to_entries | map(select(.value.metadata.type != "abstract")) | from_entries | keys | .[]' \
  2>/dev/null)

# Déploiement
for component in ${COMPONENTS}; do
  atmos terraform apply "${component}" -s "${STACK_NAME}"
done
```

## Priorité de merge Atmos

```
mixin/backend + mixin/provider
  ← catalog/_defaults
    ← blueprint (ex: containerapp-sql-access)
      ← fichier généré par CI  ← priorité maximale
```

Le fichier généré **importe** le blueprint, donc ses définitions top-level écrasent celles importées.

## Voir aussi

- [[atmos-blueprint-pattern]]
- [[atmos-describe-query]]
- [[atmos-deploy-ordering]]
- [[yq]]
