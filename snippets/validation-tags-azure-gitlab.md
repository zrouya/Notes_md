---
tags: [gitlab-ci, azure, tags, validation, snippet]
---

# Validation de tags Azure dans GitLab CI

Job GitLab CI qui valide que les tags Azure définis via des variables CI respectent une liste de valeurs autorisées.

## Variables CI attendues

| Variable | Description |
|----------|-------------|
| `AZURE_TAGS_MAP` | YAML multi-lignes `Clé: valeur` |
| `AZURE_REQUIRED_TAGS` | Liste des clés obligatoires (une par ligne) |
| `TAGS_<CLÉ>` | Valeurs autorisées pour chaque clé (une par ligne) |

## Exemple de configuration

```yaml
variables:
  AZURE_TAGS_MAP: |
    Application: monapp
    Departement: it
  AZURE_REQUIRED_TAGS: |
    Application
    Departement
  TAGS_APPLICATION: |
    monapp
    autreapp
  TAGS_DEPARTEMENT: |
    it
    rh
```

## Job complet

```yaml
check_azure_tags:
  stage: trigger
  image: alpine:latest
  before_script:
    - apk add --no-cache bash jq yq
  script:
    - |
      bash << 'EOF'
      set -euo pipefail

      errors=()

      if [ -z "${AZURE_TAGS_MAP:-}" ]; then
        errors+=("❌ AZURE_TAGS_MAP n'est pas défini ou est vide.")
      else
        if ! echo "${AZURE_TAGS_MAP}" | yq eval '.' - >/dev/null 2>&1; then
          errors+=("❌ AZURE_TAGS_MAP n'est pas un YAML valide")
        else
          if [ -z "${AZURE_REQUIRED_TAGS:-}" ]; then
            errors+=("❌ AZURE_REQUIRED_TAGS n'est pas défini ou est vide.")
          else
            mapfile -t required_keys < <(echo "${AZURE_REQUIRED_TAGS}" | tr -d '\r' | grep -v '^$')
            for key in "${required_keys[@]}"; do
              value=$(echo "${AZURE_TAGS_MAP}" | yq eval ".\"$key\"" - | sed 's/^"//;s/"$//')
              if [ -z "$value" ] || [ "$value" = "null" ]; then
                errors+=("❌ La clé '$key' est manquante ou vide dans AZURE_TAGS_MAP")
              else
                allowed_var="TAGS_${key^^}"
                allowed_raw="${!allowed_var:-}"
                if [ -z "$allowed_raw" ]; then
                  errors+=("⚠️  Variable $allowed_var non définie – impossible de valider la clé '$key'")
                else
                  allowed_values=$(echo "$allowed_raw" | tr -d '\r' | grep -v '^$')
                  if ! echo "$allowed_values" | grep -Fxq "$value"; then
                    formatted=$(echo "$allowed_values" | sed 's/^/    - /')
                    errors+=("❌ Valeur '$value' pour '$key' non autorisée.\n   Valeurs acceptées :\n${formatted}")
                  fi
                fi
              fi
            done
          fi
        fi
      fi

      if [ ${#errors[@]} -gt 0 ]; then
        echo "————————————————————————————————————————"
        echo "⚠️  Échec de la validation des tags Azure :"
        for err in "${errors[@]}"; do
          echo -e "$err"
        done
        echo "————————————————————————————————————————"
        exit 1
      fi

      echo "✅ Tous les tags Azure sont valides."
      EOF
```

## Voir aussi

- [[bash-heredoc-gitlab-ci]]
- [[mapfile]]
- [[variable-indirection]]
- [[yq]]
- [[error-accumulation-pattern]]
