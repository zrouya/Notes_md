---
tags: [gitlab-ci, pipeline, trigger, child-pipeline, multi-projet]
---

# Types de pipelines déclenchés — GitLab CI

Trois types principaux selon la relation entre les dépôts et les pipelines.

## Tableau comparatif

| Type | Même repo | Déclenche | Variables auto | Artifacts |
|---|---|---|---|---|
| Enfant (child) | Oui | Oui | Oui | Oui |
| Multi-projet | Non | Oui | Non (liste explicite) | Avec `needs` |
| Upstream (`needs: pipeline`) | Non | Non (dépendance) | N/A | Oui |

## Pipeline enfant

```yaml
trigger-child:
  trigger:
    include: child-pipeline.yml   # fichier dans le même repo
    strategy: depend
  variables:
    PASSED_VAR: "hello"
```

- Fichier YAML **dans le même dépôt**
- Utile pour découper un gros pipeline ou générer un pipeline dynamiquement
- Variables et artifacts partagés nativement

## Pipeline multi-projet

```yaml
trigger-downstream:
  trigger:
    project: group/autre-repo
    branch: main
    strategy: depend              # attend la fin du pipeline déclenché
  variables:
    MY_VAR: "value"
```

- Déclenche un pipeline dans **un autre dépôt**
- Variables à lister explicitement
- `strategy: depend` pour bloquer le pipeline déclencheur jusqu'à la fin

## Dépendance upstream (`needs: pipeline`)

```yaml
job:
  needs:
    - pipeline: group/autre-repo
      job: build
```

- Consomme des **artifacts** d'un pipeline externe déjà lancé
- Pas un déclenchement, c'est une dépendance

## Voir aussi

- [[pipeline-downstream-variables]]
- [[pattern-iac-centralise]]
