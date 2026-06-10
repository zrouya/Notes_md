---
tags: [git, merge, fast-forward, historique]
---

# git fast-forward — avancer le pointeur sans merge

Cas le plus simple d'une fusion : quand la branche cible **n'a pas divergé**, Git ne crée aucun commit de merge, il **fait juste glisser le pointeur de branche en avant**.

## Condition de déclenchement

Fast-forward possible ⟺ la branche cible est un **ancêtre direct** de la branche source (chemin droit, pas de divergence).

```
  D---E---F        (main)        →  git merge feature  →   D---E---F---A---B---C  (main, feature)
           \
            A---B---C  (feature)
```

`main` (sur `F`) est ancêtre de `C` → le pointeur `main` glisse de `F` à `C`. **Aucun nouveau commit.**

## Impossible si divergence

Si `main` a avancé (commit `G`), `F` n'est plus son extrémité → Git doit créer un **commit de merge** à 2 parents (merge à 3 voies). Voir [[git-rebase-vs-merge]].

## Contrôler le comportement

| Option | Comportement |
|--------|--------------|
| `--ff` (défaut) | fast-forward si possible, sinon commit de merge |
| `--no-ff` | force un commit de merge même si un FF était possible |
| `--ff-only` | échoue si le FF est impossible (refuse de créer un merge) |

```bash
git merge --no-ff feature   # garde une "bulle" visible de la feature
git merge --ff-only feature # garantit un historique linéaire, sinon erreur
git pull --ff-only          # pull sans merge surprise
```

## Combo historique linéaire

```bash
git switch feature
git rebase main      # supprime la divergence → feature redevient "devant" main
git switch main
git merge feature    # devient un simple fast-forward
```

## Voir aussi

- [[git-rebase-vs-merge]]
- [[git-rebase]]
- [[git-switch]]
