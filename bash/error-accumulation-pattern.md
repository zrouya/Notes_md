---
tags: [bash, patterns, erreurs]
---

# Pattern d'accumulation d'erreurs bash

Collecte toutes les erreurs avant de quitter, plutôt que d'échouer à la première. Utile pour des validations multi-étapes.

## Syntaxe / Exemple

```bash
set -euo pipefail

errors=()

# Accumuler les erreurs
if [ -z "${MY_VAR:-}" ]; then
  errors+=("❌ MY_VAR est vide")
fi

if ! some_check; then
  errors+=("❌ some_check a échoué")
fi

# Afficher et quitter si des erreurs existent
if [ ${#errors[@]} -gt 0 ]; then
  echo "————————————————————————————"
  echo "⚠️  Erreurs de validation :"
  for err in "${errors[@]}"; do
    echo -e "$err"
  done
  echo "————————————————————————————"
  exit 1
fi

echo "✅ Validation réussie."
```

## Détails

- `errors=()` : initialise un tableau vide
- `errors+=("message")` : ajoute un élément
- `${#errors[@]}` : nombre d'éléments dans le tableau
- `echo -e` : interprète les séquences d'échappement (`\n`, etc.)
- Compatible avec `set -e` car l'exit explicite est dans le bloc conditionnel

## Voir aussi

- [[set-euo-pipefail]]
- [[mapfile]]
