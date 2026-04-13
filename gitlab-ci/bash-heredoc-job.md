---
tags: [gitlab-ci, bash, heredoc]
---

# Heredoc bash dans un job GitLab CI

Permet d'écrire un script bash multi-lignes avec `set -euo pipefail` dans un job GitLab CI, sans les problèmes d'interprétation du YAML.

## Syntaxe / Exemple

```yaml
mon-job:
  stage: build
  image: alpine:latest
  before_script:
    - apk add --no-cache bash jq yq
  script:
    - |
      bash << 'EOF'
      set -euo pipefail

      echo "Ceci s'exécute dans bash strict"
      # ... reste du script
      EOF
```

## Détails

- `bash << 'EOF'` : heredoc avec guillemets simples — les `$VAR` ne sont **pas** interprétés par le shell YAML, mais bien par bash au moment de l'exécution
- `|` avant le bloc : préserve les sauts de ligne dans le YAML (scalaire littéral)
- `set -euo pipefail` doit être placé **à l'intérieur** du heredoc pour être effectif
- Les variables GitLab CI (`$CI_VAR`) restent accessibles à l'intérieur

**Pourquoi le heredoc et pas juste `script: - |` ?**  
Le heredoc garantit que bash est utilisé (et non `sh`/`dash`), ce qui est nécessaire pour les features bash 4+ (`mapfile`, `${!var}`, `${var^^}`, etc.).

## Voir aussi

- [[set-euo-pipefail]]
- [[mapfile]]
- [[variable-indirection]]
