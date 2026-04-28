---
tags: [iac, atmos, gitlab-ci, ordonnancement]
---

# Atmos — Ordonnancement des composants en CI

Trois stratégies pour contrôler l'ordre de déploiement des composants d'une stack.

## Contexte

`atmos describe stacks --query` retourne les composants sans ordre garanti.  
Si des composants ont des dépendances (ex: DB avant règles d'accès), l'ordre doit être géré explicitement.

## Option A — `deploy_order` dans le fichier projet (recommandé)

Le projet déclare l'ordre dans son `atmos-config.yaml` :

```yaml
# atmos-config.yaml
deploy_order:
  - sqlDatabase/new-db
  - sqlAppAccess/ca1-to-new-db
  - sqlAppAccess/ca2-to-new-db

components:
  terraform:
    sqlDatabase/new-db: ...
    sqlAppAccess/ca1-to-new-db: ...
```

Le CI lit `deploy_order` en priorité, sinon découvre via `describe stacks --query` :

```bash
# Lire deploy_order si présent, sinon auto-découverte
ORDERED=$(yq eval '.deploy_order[]' atmos-config.yaml 2>/dev/null)
if [ -z "$ORDERED" ]; then
  ORDERED=$(atmos describe stacks --stack "${STACK_NAME}" \
    --query '.[].components.terraform | to_entries | map(select(.value.metadata.type != "abstract")) | from_entries | keys | .[]' \
    2>/dev/null)
fi
```

**Avantages :** simple, lisible, le projet contrôle son ordre sans connaître Atmos.

## Option B — Atmos Workflows

Définir un workflow ordonné dans un fichier YAML :

```yaml
# stacks/workflows/deploy.yaml
workflows:
  deploy-all:
    steps:
      - command: terraform apply sqlDatabase/new-db -s myapp-dev
      - command: terraform apply sqlAppAccess/ca1-to-new-db -s myapp-dev
```

```bash
atmos workflow deploy-all --file stacks/workflows/deploy.yaml
```

**Avantages :** natif Atmos, pas de code CI custom.  
**Inconvénient :** le fichier workflow doit être injecté ou versionné dans le repo IaC central.

## Option C — `settings.depends_on` + tri topologique

Déclarer les dépendances dans les catalog defaults, puis construire un graphe orienté en CI.  
Voir [[atmos-describe-dependents]] pour la syntaxe.

**Avantages :** robuste, auto-calculé.  
**Inconvénient :** investissement initial important, code de tri topologique à maintenir.

## Recommandation

- **Stacks simples** → Option A (`deploy_order`)
- **Stacks complexes récurrentes** → Option B (Workflows)
- **Beaucoup de stacks avec dépendances variées** → Option C

## Voir aussi

- [[atmos-describe-dependents]]
- [[atmos-ci-dynamic-stack]]
- [[atmos-describe-query]]
