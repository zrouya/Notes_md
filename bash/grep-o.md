---
tags: [bash, grep, parsing]
---

# grep -o — n'imprimer que la correspondance

`grep -o` affiche uniquement la portion du texte qui correspond au pattern, pas la ligne entière.

## Syntaxe

```bash
echo "texte" | grep -o 'pattern'
```

## Exemple : extraire une valeur JSON sans jq

Input : `..."container_name": "passthrough-myapi",...`

```bash
# Étape 1 — extraire la paire clé/valeur
echo "$JSON" | grep -o '"container_name"[[:space:]]*:[[:space:]]*"[^"]*"'
# → "container_name": "passthrough-myapi"

# Étape 2 — isoler la valeur (dernière chaîne entre guillemets en fin de ligne)
... | grep -o '"[^"]*"$'
# → "passthrough-myapi"

# Étape 3 — supprimer les guillemets
... | tr -d '"'
# → passthrough-myapi
```

## Pipeline complet

```bash
VALUE=$(echo "$JSON" \
  | grep -o '"container_name"[[:space:]]*:[[:space:]]*"[^"]*"' \
  | head -1 \
  | grep -o '"[^"]*"$' \
  | tr -d '"')
```

- `head -1` : garde la première occurrence si la clé apparaît plusieurs fois

## Voir aussi

- [[regex-posix]]
- [[tr]]
