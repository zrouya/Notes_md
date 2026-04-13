---
tags: [bash, robustesse, options]
---

# set -euo pipefail — Mode strict bash

Active trois protections essentielles pour des scripts bash robustes.

## Syntaxe / Exemple

```bash
set -euo pipefail

# ou en shebang one-liner
#!/usr/bin/env bash
set -euo pipefail
```

## Détails

| Option | Effet |
|--------|-------|
| `-e`   | Quitte immédiatement si une commande échoue (exit code ≠ 0) |
| `-u`   | Erreur si une variable non définie est utilisée |
| `-o pipefail` | Échoue si n'importe quelle commande dans un pipe échoue (pas seulement la dernière) |

**Valeur par défaut safe pour une variable potentiellement non définie** (contourne `-u`) :
```bash
MY_VAR="${MY_VAR:-}"        # vide par défaut
MY_VAR="${MY_VAR:-valeur}"  # valeur par défaut
```

## Voir aussi

- [[mapfile]]
- [[error-accumulation-pattern]]
