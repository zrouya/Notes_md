---
tags: [git, rebase, historique]
---

# git rebase -i — réécrire / nettoyer ses commits

Rejoue une série de commits en permettant de les **transformer au passage** : fusionner, supprimer, renommer, réordonner. Sert à « nettoyer ses commits » avant une PR (rendre chaque commit autoporteur).

## Syntaxe / Exemple

```bash
git rebase -i HEAD~6   # éditer les 6 derniers commits
git rebase -i main     # rejouer sur main ET nettoyer en une fois
```

Git ouvre un éditeur listant les commits + une action par ligne.

## Actions disponibles

| Action | Effet |
|--------|-------|
| `pick` | garder tel quel |
| `reword` | garder, modifier le message |
| `edit` | s'arrêter pour modifier le contenu |
| `squash` | fusionner avec le précédent (garde les 2 messages) |
| `fixup` | fusionner avec le précédent (jette son message) |
| `drop` | supprimer le commit |
| réordonner | changer l'ordre des lignes |

## Exemple de nettoyage

```
pick   a1  Ajout du parser JSON
fixup  b2     (fix typo → fusionné dans a1)
fixup  c3     (oups fichier oublié → fusionné dans a1)
pick   d4  Ajout des tests
fixup  f6     (re-fix typo, remonté sous d4)
drop   e5     (debug print supprimé)
```

→ 2 commits propres. **Code final identique**, histoire devenue lisible.

## ⚠️ Bloquant

`-i` ouvre un éditeur interactif → inutilisable en contexte automatisé/CI sans pilotage manuel. Réécrit l'historique → voir règle d'or de [[git-rebase]].

## Voir aussi

- [[git-rebase]]
- [[git-push-force-with-lease]]
