---
tags: [moc, git]
---

# Git — Map of Content

Notes sur l'usage quotidien de git.

## Branches

- [[git-switch]] — changer de branche (remplace `git checkout` pour ce rôle)
- [[git-switch-create-branch]] — créer une branche locale depuis une branche/réf source
- [[git-push-set-upstream]] — créer la branche distante + suivi (`-u`, `push.autoSetupRemote`)
- [[git-checkout-vs-switch]] — scission de `checkout` en `switch` (branches) + `restore` (fichiers)
- [[git-restore]] — restaurer fichiers & index (rejeter modifs, dé-stager, `--source`)

## Merge

- [[git-fast-forward]] — avancer le pointeur sans commit de merge (`--ff` / `--no-ff` / `--ff-only`)

## Rebase & historique

- [[git-rebase]] — re-baser une série de commits (rejeu, hashes recalculés, règle d'or)
- [[git-rebase-vs-merge]] — historique linéaire vs commit de merge
- [[git-rebase-interactif]] — `rebase -i` : squash/fixup/drop/reword pour nettoyer ses commits
- [[git-pull-rebase]] — récupérer le distant sans commit de merge
- [[git-push-force-with-lease]] — pousser après réécriture d'historique sans écraser autrui
- [[git-reflog]] — filet de sécurité pour revenir à un état précédant un rebase
