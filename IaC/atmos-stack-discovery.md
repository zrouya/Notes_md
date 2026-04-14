---
tags: [iac, atmos, configuration]
---

# Atmos — Découverte des stacks

Atmos détermine quels fichiers YAML sont des entry points de stacks via `included_paths` et `excluded_paths`.

## Configuration (atmos.yaml)

```yaml
stacks:
  base_path: "./stacks"
  included_paths:
    - "templates/**/*.yaml"   # tous les .yaml sous templates/
  excluded_paths:
    - "**/_defaults.yaml"     # fichiers de defaults (convention _)
    - "**/project-config/**"  # dossier de configs projet
  name_template: "{{.vars.template}}-{{.vars.environment}}"
```

## Règles importantes

- **Tout fichier** dans `included_paths` et hors `excluded_paths` est traité comme une stack
- Un fichier non exclu avec des Go templates qui cassent le YAML → erreur `failed to parse YAML for locals extraction`
- Deux fichiers produisant le même nom de stack → conflit

## Patterns utiles

```yaml
excluded_paths:
  - "**/_defaults.yaml"       # fichiers commençant par _
  - "**/project-config/**"    # tout un sous-dossier
  - "**/project-config.yaml"  # un fichier spécifique (tous dossiers)
```

## `name_template` vs `name_pattern`

- `name_template` : Go template — `"{{.vars.template}}-{{.vars.environment}}"`
- `name_pattern` : pattern positionnel (ancien format)
- Si `name_template` échoue à s'évaluer, Atmos cherche `name_pattern` → erreur si absent

## Fichiers project-config (pattern recommandé)

```
stacks/
└── templates/
    ├── project-config/           ← exclu de la découverte
    │   └── *_project-config.yaml ← importé par les stack files
    └── frontwebapp/
        └── dev.yaml              ← stack entry point, importe project-config
```

```yaml
# dev.yaml
import:
  - "./_defaults.yaml"
  - "../project-config/mon-app_project-config.yaml"
```

## Voir aussi

- [[atmos-overview]]
- [[atmos-stack-composition]]
