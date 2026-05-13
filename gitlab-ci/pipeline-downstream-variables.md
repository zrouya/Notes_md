---
tags: [gitlab-ci, pipeline, variables, trigger]
---

# Transmettre des variables à un pipeline downstream

Deux mécanismes selon le besoin : passer des valeurs explicites ou hériter des variables courantes.

## Syntaxe / Exemple

```yaml
# Passer des variables explicites
trigger-downstream:
  trigger:
    project: group/downstream-project
    branch: main
  variables:
    MY_VAR: "value"
    BRANCH: $CI_COMMIT_REF_NAME   # transmettre une variable courante

# Héritage sélectif
trigger-downstream:
  trigger:
    project: group/downstream-project
  inherit:
    variables:
      - MY_VAR
      - ANOTHER_VAR

# Tout transmettre (défaut GitLab 16.x+)
inherit:
  variables: true

# Ne rien transmettre
inherit:
  variables: false
```

## Détails

- `variables:` sous `trigger:` → valeurs explicites, prioritaires
- `inherit: variables:` → liste blanche depuis le pipeline courant
- Par défaut (multi-projet), les variables **ne sont pas** transmises automatiquement
- Pour un pipeline enfant (même repo), elles le sont par défaut

## Voir aussi

- [[types-pipelines-declenches]]
- [[pattern-iac-centralise]]
