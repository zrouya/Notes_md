---
tags: [bash, arrays, input]
---

# mapfile (readarray)

Lit des lignes depuis stdin et les stocke dans un tableau bash. Alias : `readarray`.

## Syntaxe / Exemple

```bash
# Lire depuis une commande (process substitution)
mapfile -t my_array < <(echo -e "ligne1\nligne2\nligne3")

# Lire depuis une variable multi-lignes
mapfile -t keys < <(echo "${MY_VAR}" | tr -d '\r' | grep -v '^$')

for key in "${keys[@]}"; do
  echo "$key"
done
```

## Détails

- `-t` : supprime le `\n` de fin de chaque ligne (presque toujours souhaité)
- `< <(...)` : process substitution — évite de lancer mapfile dans un sous-shell
- `tr -d '\r'` : nettoie les fins de ligne Windows (CRLF → LF)
- `grep -v '^$'` : ignore les lignes vides

## Voir aussi

- [[set-euo-pipefail]]
- [[error-accumulation-pattern]]
