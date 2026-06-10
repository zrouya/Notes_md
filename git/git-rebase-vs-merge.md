---
tags: [git, rebase, merge, historique]
---

# rebase vs merge — intégrer les nouveautés d'une branche

Deux façons d'intégrer l'avancée de `main` dans `feature`. Différence = **forme du graphe**, pas le contenu des commits.

## merge — conserve la divergence

```bash
git switch feature
git merge main
```

```
        A---B---C---M   (feature)
       /           /
  D---E---F---G----    (main)
```

`M` = commit de merge : aucun travail à vous, juste un nœud technique. Mergé plusieurs fois → graphe entrelacé, `git log --graph` illisible, `git bisect` pénible.

## rebase — historique linéaire

```bash
git switch feature
git rebase main
```

```
              A'--B'--C'   (feature)
             /
  D---E---F---G            (main)
```

Pas de commit `M`. La branche apparaît comme commencée depuis le dernier état de `main` → ligne droite, lisible. C'est ça « historique propre » : **forme** linéaire, sans nœuds de fusion parasites (le contenu de A'/B'/C' est inchangé).

## Voir aussi

- [[git-rebase]]
- [[git-pull-rebase]]
