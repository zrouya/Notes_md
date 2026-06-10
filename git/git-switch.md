---
tags: [git, branche, switch]
---

# git switch — changer de branche

Commande dédiée au **changement de branche** (introduite en Git 2.23 pour séparer ce rôle de `git checkout`, qui faisait trop de choses à la fois).

## Syntaxe / Exemple

```bash
git switch main                 # basculer sur une branche existante
git switch -c feature/x         # créer + basculer (voir note dédiée)
git switch -                    # revenir à la branche précédente
git switch -d <commit|tag>      # se détacher sur un commit (detached HEAD)
git switch -c hotfix origin/dev # créer depuis une réf distante
```

## Détails

- `switch` ne gère **que** les branches → moins ambigu que `checkout`.
- `git checkout` reste valide mais surchargé : il sert aussi à restaurer des fichiers (rôle désormais dévolu à `git restore`).
- Refuse de basculer si des modifications non commitées seraient écrasées → committer ou `git stash` d'abord.
- `-d` (detached HEAD) : on est sur un commit sans branche ; tout nouveau commit y sera orphelin tant qu'on ne crée pas de branche.

## Équivalences checkout

| But | switch | checkout (ancien) |
|-----|--------|-------------------|
| Changer de branche | `git switch x` | `git checkout x` |
| Créer + basculer | `git switch -c x` | `git checkout -b x` |
| Branche précédente | `git switch -` | `git checkout -` |

Détail complet de la scission `checkout` → `switch` + `restore` : [[git-checkout-vs-switch]].

## Voir aussi

- [[git-checkout-vs-switch]]
- [[git-restore]]
- [[git-switch-create-branch]]
- [[git-push-set-upstream]]
- [[git-fast-forward]]
