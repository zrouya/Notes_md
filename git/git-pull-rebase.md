---
tags: [git, rebase, pull, remote]
---

# git pull --rebase — récupérer sans commit de merge

Au lieu de merger les commits distants (ce que fait `git pull` par défaut), **rejoue vos commits locaux par-dessus** les commits récupérés → évite les commits de merge inutiles.

## Syntaxe / Exemple

```bash
git pull --rebase

# par défaut, une fois pour toutes :
git config --global pull.rebase true
```

## Détails

- `git pull` = `fetch` + `merge` → crée un commit de merge si vos commits locaux divergent.
- `git pull --rebase` = `fetch` + `rebase` → historique local linéaire, pas de nœud de fusion.
- S'appuie sur le même moteur que [[git-rebase]] (rejeu de commits, hashes recalculés).

## Voir aussi

- [[git-rebase]]
- [[git-rebase-vs-merge]]
