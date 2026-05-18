---
tags: [bash, regex, grep]
---

# Regex POSIX dans grep

Éléments de regex utilisables avec `grep` (et `sed`, `awk`).

## Classes de caractères POSIX

| Classe | Signification |
|--------|---------------|
| `[[:space:]]` | Espace, tabulation, retour chariot… |
| `[[:alpha:]]` | Lettres a-z et A-Z |
| `[[:digit:]]` | Chiffres 0-9 |
| `[[:alnum:]]` | Lettres + chiffres |

## Quantificateurs

| Symbole | Signification |
|---------|---------------|
| `*` | Zéro ou plusieurs fois le caractère/groupe précédent |
| `+` | Une ou plusieurs fois (ERE : `grep -E` ou `grep -P`) |
| `?` | Zéro ou une fois (ERE) |

## Ancres et négation

| Symbole | Signification |
|---------|---------------|
| `$` | Fin de ligne |
| `^` | Début de ligne |
| `[^x]` | Tout caractère **sauf** `x` |

## Exemples typiques

```bash
# Zéro ou plusieurs espaces autour d'un ":"
[[:space:]]*:[[:space:]]*

# Tout caractère sauf un guillemet (valeur JSON)
[^"]*

# Chaîne entre guillemets en fin de ligne
"[^"]*"$
```

## Voir aussi

- [[grep-o]]
