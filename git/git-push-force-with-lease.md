---
tags: [git, push, rebase, remote]
---

# git push --force-with-lease — pousser après réécriture d'historique

Force la mise à jour d'une branche distante dont l'historique a été réécrit (rebase, amend), **sans écraser** le travail d'un collègue arrivé entre-temps.

## Syntaxe / Exemple

```bash
git push --force-with-lease
```

## Pourquoi

Après un [[git-rebase]] / `--amend`, les hashes locaux diffèrent du distant → `git push` normal refusé (l'historique distant n'est plus un ancêtre du local).

- `--force` brut : écrase le distant aveuglément (risque de perdre des commits poussés par d'autres).
- `--force-with-lease` : refuse de pousser si le distant a bougé depuis votre dernier `fetch` → filet de sécurité. **À préférer systématiquement.**

## Voir aussi

- [[git-rebase]]
- [[git-rebase-interactif]]
- [[git-reflog]]
