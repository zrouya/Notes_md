---
tags: [bash, variables, expansion]
---

# Indirection de variables et expansion de casse

Deux expansions bash avancées souvent utilisées ensemble pour construire des noms de variables dynamiques.

## Syntaxe / Exemple

```bash
# ${!var} — indirection : utilise la valeur de $var comme nom de variable
key="APPLICATION"
allowed_var="TAGS_${key}"       # TAGS_APPLICATION
value="${!allowed_var:-}"       # lit $TAGS_APPLICATION

# ${var^^} — convertit en MAJUSCULES
key="application"
upper="${key^^}"   # APPLICATION

# ${var,,} — convertit en minuscules
lower="${key,,}"   # application

# Combiné : construire un nom de variable dynamique depuis une clé
key="Application"
allowed_var="TAGS_${key^^}"           # TAGS_APPLICATION
allowed_raw="${!allowed_var:-}"       # contenu de $TAGS_APPLICATION
```

## Détails

- `${!var}` : indirection — Bash 2+, pas disponible en `sh` ou `dash`
- `${var^^}` / `${var,,}` : expansion de casse — Bash 4+
- Utile pour des systèmes de configuration où les noms de variables sont construits dynamiquement

## Voir aussi

- [[set-euo-pipefail]]
- [[mapfile]]
