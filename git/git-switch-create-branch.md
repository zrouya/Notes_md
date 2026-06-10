---
tags: [git, branche]
---

# git switch -c — créer une branche depuis une source

Crée une nouvelle branche locale et bascule dessus, en partant d'un point de départ explicite (idéalement une réf distante à jour).

## Syntaxe / Exemple

```bash
git fetch origin
git switch -c feature/ma-branche origin/dev
# ancienne syntaxe équivalente :
git checkout -b feature/ma-branche origin/dev
```

## Détails

- `-c` = create + switch.
- Partir de `origin/dev` (et non `dev`) garantit le dernier état distant, pas un local périmé → d'où le `git fetch` préalable.
- Le point de départ peut être une branche, un tag ou un commit.

## Piège : suivi auto vers la branche source

Quand on part d'une **branche distante**, git affiche `set up to track 'origin/dev'` et définit `origin/dev` comme **upstream** de la nouvelle branche (défaut `branch.autoSetupMerge`).

Conséquences :
- `git status` / `git pull` se comparent à `origin/dev` (souvent voulu pour rester à jour).
- `git push` sans `-u` viserait `origin/dev` ⚠️ (mais `push.default = simple`, le défaut moderne, refuse si nom local ≠ nom upstream → protège d'un push accidentel sur dev).

Pour relier le suivi à **sa propre** branche distante, pousser avec `-u` → voir [[git-push-set-upstream]].

Vérifier l'upstream à tout moment :

```bash
git branch -vv   # upstream affiché entre crochets : [origin/dev] puis [origin/feature/...]
```

## Voir aussi

- [[git-push-set-upstream]]
