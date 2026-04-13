---
tags: [outils, yaml, cli]
---

# yq — Processeur YAML en ligne de commande

Équivalent de `jq` pour le YAML. Permet de lire, valider et transformer du YAML depuis le terminal.

## Syntaxe / Exemple

```bash
# Valider qu'un YAML est syntaxiquement correct
echo "$YAML_VAR" | yq eval '.' - >/dev/null 2>&1

# Extraire la valeur d'une clé
value=$(echo "$YAML_VAR" | yq eval '."Application"' -)

# Extraire une clé dont le nom est dans une variable
key="Application"
value=$(echo "$YAML_VAR" | yq eval ".\"$key\"" -)

# Nettoyer les guillemets éventuels dans la sortie
value=$(echo "$YAML_VAR" | yq eval ".\"$key\"" - | sed 's/^"//;s/"$//')
```

## Détails

- `-` en dernier argument : lire depuis stdin
- `eval '.'` : affiche le YAML tel quel (validation)
- Retourne `null` si une clé est absente — tester avec `[ "$value" = "null" ]`
- Installation Alpine : `apk add --no-cache yq`

## Voir aussi

- [[bash-heredoc-gitlab-ci]]
- [[validation-tags-azure-gitlab]]
