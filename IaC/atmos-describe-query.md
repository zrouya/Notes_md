---
tags: [iac, atmos, cli, yq]
---

# Atmos — `describe stacks --query`

Filtrer la sortie de `atmos describe stacks` avec une expression **yq** pour extraire programmatiquement des valeurs résolues.

## Syntaxe / Exemple

```bash
# Lister tous les composants concrets (non abstraits) d'une stack
atmos describe stacks --stack myapp-dev \
  --query '.[].components.terraform | to_entries | map(select(.value.metadata.type != "abstract")) | from_entries | keys | .[]'

# Récupérer les vars résolues d'un composant
atmos describe stacks --stack myapp-dev \
  --query '.[].components.terraform["sqlAppAccess/ca1-to-db1"].vars'

# Récupérer les metadata d'un composant
atmos describe stacks --stack myapp-dev \
  --query '.[].components.terraform["sqlAppAccess/defaults"].metadata'
```

## Structure de la sortie brute

```yaml
myapp-dev:                          # clé = nom de la stack
  components:
    terraform:
      sqlAppAccess/ca1-to-db1:
        metadata:
          type: abstract | ""       # absent si concret
        vars: ...
```

Toutes les expressions commencent donc par `.[].` pour descendre sous la clé de nom de stack.

## Détails

- La sortie est **résolue** : imports et `inherits` sont déjà fusionnés.
- Les composants abstraits ont `metadata.type: "abstract"`.
- Utile en CI pour découvrir dynamiquement les composants à déployer.
- Syntaxe yq, pas jq — quelques différences subtiles sur les pipes et filtres.

## Voir aussi

- [[atmos-stack-discovery]]
- [[atmos-ci-dynamic-stack]]
- [[yq]]
