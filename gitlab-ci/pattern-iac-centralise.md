---
tags: [gitlab-ci, iac, trigger, multi-projet, architecture]
---

# Pattern — Repos applicatifs → Repo IaC centralisé

Centraliser les jobs de déploiement IaC dans un seul repo, déclenché par les repos applicatifs.

## Approches disponibles

### 1. `include:` remote *(simple, couplé)*

```yaml
# pipeline.yml du repo applicatif
include:
  - project: 'group/iac-repo'
    ref: main
    file: '/iac/pipeline.yml'
```

- Jobs fusionnés dans le pipeline de l'app
- Variables CI/CD de l'app disponibles nativement
- Attention : le code IaC doit être accessible depuis le runner (clone ou sous-dossier)
- Risque : `ref: main` = impact immédiat de tout changement IaC sur toutes les apps

### 2. Multi-projet `trigger:` *(recommandé)*

```yaml
# pipeline.yml du repo applicatif
deploy-iac:
  trigger:
    project: group/iac-repo
    branch: main
    strategy: depend
  variables:
    ATMOS_COMPONENT: containerApp
    ATMOS_TEMPLATE: myapp
    ATMOS_PROJECT_NAME: $CI_PROJECT_NAME
```

- Secrets Azure centralisés dans le repo IaC uniquement
- Séparation claire app / infra
- Versioning indépendant possible (`ref: v1.2`)
- Variables à lister explicitement

### 3. Git submodule *(lourd)*

- IaC comme submodule dans chaque app sous `/iac/`
- Gestion des submodules lourde à maintenir

### 4. Compliance Pipelines *(GitLab Premium)*

- Pipeline imposé automatiquement à tous les projets d'un groupe
- Zéro configuration côté app, mais nécessite Premium

## Recommandation

**Option 2 (multi-projet)** pour :
- Centraliser les secrets Azure/cloud
- Garder un versioning stable du pipeline IaC
- Éviter que les devs app touchent à la config infra

## Prérequis pour l'option 2

Le repo IaC doit être **autonome** : cloner ses propres sources dans les jobs, sans dépendre d'un `CI_PROJECT_DIR` externe.

## Voir aussi

- [[types-pipelines-declenches]]
- [[pipeline-downstream-variables]]
