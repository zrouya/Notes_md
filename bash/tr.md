---
tags: [bash, tr, texte]
---

# tr — transformation de caractères

`tr` lit stdin et remplace ou supprime des caractères.

## Syntaxe

```bash
echo "texte" | tr [OPTIONS] set1 [set2]
```

## Options courantes

| Option | Effet |
|--------|-------|
| `-d` | **Supprime** tous les caractères de `set1` |
| `-s` | Compresse les répétitions de `set1` en un seul caractère |

## Exemples

```bash
# Supprimer les guillemets
echo '"passthrough-myapi"' | tr -d '"'
# → passthrough-myapi

# Mettre en majuscules
echo "hello" | tr 'a-z' 'A-Z'
# → HELLO

# Supprimer les retours à la ligne
echo -e "a\nb" | tr -d '\n'
# → ab
```

## Voir aussi

- [[grep-o]]
