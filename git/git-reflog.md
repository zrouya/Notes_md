---
tags: [git, reflog, récupération]
---

# git reflog — filet de sécurité après réécriture

Journal de tous les déplacements de `HEAD` (commits, resets, rebases, checkouts). Permet de revenir à un état précédant un rebase « raté ».

## Syntaxe / Exemple

```bash
git reflog
# 7g8h9i HEAD@{2}: rebase: ...
# a1b2c3 HEAD@{3}: commit: mon état avant rebase

git reset --hard HEAD@{3}   # revenir à l'état d'avant
```

## Détails

- Rien n'est perdu immédiatement après un rebase : les anciens commits restent accessibles via le reflog.
- Ils survivent tant que le garbage collector ne les nettoie pas (~90 jours par défaut).
- Indispensable comme garde-fou des opérations qui réécrivent l'historique.

## Voir aussi

- [[git-rebase]]
- [[git-rebase-interactif]]
