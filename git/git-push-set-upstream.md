---
tags: [git, branche, remote]
---

# git push -u — créer la branche distante + suivi

Pousse une branche locale, crée la branche distante correspondante **et** établit le lien de suivi (upstream) en une commande.

## Syntaxe / Exemple

```bash
git push -u origin feature/ma-branche
# -u = --set-upstream
```

Ensuite un simple `git push` / `git pull` suffit (plus besoin de préciser remote/branche).

## Détails

- Sans `-u`, la branche distante n'est pas liée → `git push` seul échoue tant que l'upstream n'est pas défini.
- Astuce config (une fois pour toutes) :
  ```bash
  git config --global push.autoSetupRemote true
  ```
  → un simple `git push` crée alors la branche distante + le suivi automatiquement, sans `-u`.

## Voir aussi

- [[git-switch-create-branch]]
