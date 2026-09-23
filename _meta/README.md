# _meta

Fichiers de configuration/tooling pour ce vault — pas des notes de contenu, donc hors de la structure par domaines et hors des MOC.

## `claude-commands/`

Copie versionnée des définitions de commandes slash Claude Code utilisées pour alimenter ce vault (ex. `/obsidian`). La copie "active" que Claude Code exécute réellement vit hors du repo, dans `~/.claude/commands/` sur chaque machine — ce dossier sert à la porter d'une machine à l'autre.

**Pour installer/mettre à jour une commande sur une nouvelle machine :**
```bash
cp "_meta/claude-commands/obsidian.md" ~/.claude/commands/obsidian.md
```
(sur Windows : copier le fichier vers `C:\Users\<user>\.claude\commands\obsidian.md`, ou l'équivalent `%USERPROFILE%\.claude\commands\`)

Si tu modifies la commande via une session Claude Code (ex. "mets à jour la skill avec..."), l'édition se fait sur la copie active `~/.claude/commands/`. Pense à recopier vers `_meta/claude-commands/` (et committer) pour que le changement se propage aux autres machines.
