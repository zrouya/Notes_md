---
tags: [git, rebase, historique]
---

# git rebase — re-baser une série de commits

Donne une nouvelle **base** (nouveau commit parent de départ) à une série de commits, en les rejouant un par un par-dessus cette base.

## Syntaxe / Exemple

```bash
# depuis la branche feature, la rejouer par-dessus main à jour
git switch feature
git rebase main
```

## Mécanique

1. Calcule l'**ancêtre commun** entre la branche et la base.
2. Identifie les commits propres à la branche depuis cet ancêtre.
3. Déplace HEAD sur la nouvelle base.
4. **Rejoue chaque commit** : applique son diff → crée un **nouveau commit** (A → A').
5. Déplace le pointeur de branche sur le dernier commit rejoué.

## Pourquoi les hashes changent

Un commit contient un pointeur vers son **parent**. Son hash SHA-1 dépend du parent → changer la base = nouveau parent = **nouveaux hashes** (A', B', C'), même à contenu identique.

## Conflits & contrôle

```bash
git add <fichiers>
git rebase --continue   # poursuit le rejeu après résolution
git rebase --skip       # ignore le commit en cours
git rebase --abort      # annule tout, retour à l'état initial
```

## Règle d'or ⚠️

Ne **jamais** rebaser des commits déjà partagés/poussés sur une branche utilisée par d'autres → réécriture d'historique = divergence inextricable. Voir [[git-push-force-with-lease]].

## Voir aussi

- [[git-rebase-vs-merge]]
- [[git-rebase-interactif]]
- [[git-pull-rebase]]
- [[git-reflog]]
