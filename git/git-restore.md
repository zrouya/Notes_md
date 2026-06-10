---
tags: [git, restore, fichiers, index]
---

# git restore — restaurer fichiers & index

Commande dédiée à la **manipulation du contenu des fichiers** (Git 2.23), reprenant les rôles « fichiers » de `git checkout` et `git reset`. Distingue explicitement le *working tree* et l'*index*.

## Syntaxe / Exemple

```bash
git restore fichier.txt                 # rejette les modifs du working tree
git restore --staged fichier.txt        # dé-stage (= git reset HEAD fichier)
git restore --staged --worktree f.txt   # les deux à la fois
git restore --source=abc123 fichier.txt # restaure depuis un commit précis
```

## Détails

- Par défaut, agit sur le **working tree** (`--worktree`, implicite).
- `--staged` agit sur l'**index** (zone de staging) → utile pour annuler un `git add`.
- `--source=<ref>` choisit la version source (commit, tag, branche) ; défaut = index, sinon HEAD.
- ⚠️ `git restore f` **écrase les modifs non commitées** sans récupération possible (comme `checkout -- f`). L'intention est ici explicite, contrairement à l'ancien `checkout`.

## Voir aussi

- [[git-checkout-vs-switch]]
- [[git-switch]]
