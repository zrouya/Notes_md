# /obsidian — Alimenter le vault Obsidian

## Étape 0 — Trouver le vault

Lis le fichier `~/.claude/obsidian-vault` (via le tool Read, chemin complet : `C:/Users/zrouya.externe/.claude/obsidian-vault` sur le PC pro, mais utilise toujours Read sur `~/.claude/obsidian-vault` pour être portable).

En pratique : utilise le tool Read sur le chemin `~/.claude/obsidian-vault` — la première ligne contient le chemin absolu du vault à utiliser. Si le fichier est absent, informe l'utilisateur qu'il doit le créer avec `echo "C:\chemin\vers\vault" > ~/.claude/obsidian-vault`.

## Ta mission

Passe en revue l'échange en cours et identifie tout ce qui mérite d'être capitalisé :
commandes shell, concepts techniques, snippets, tips, outils, workflows GitLab CI, etc.

## Règles de génération

### Structure des fichiers
- **Un fichier par concept atomique** : `mapfile.md`, `yq-eval.md`, `set-euo-pipefail.md`
- **Fichiers courts** : 20-60 lignes max. Si un sujet est large, découpe-le.
- **Nommage** : kebab-case, en français ou en anglais selon ce qui est le plus naturel pour le concept

### Format de chaque note
```
---
tags: [tag1, tag2]
---

# Titre du concept

Courte description (1-2 phrases max).

## Syntaxe / Exemple

\`\`\`bash
exemple de code
\`\`\`

## Détails

Explication concise. Évite la prose longue, préfère les listes.

## Voir aussi

- [[note-liée-1]]
- [[note-liée-2]]
```

### Liens Obsidian
- Utilise `[[nom-du-fichier]]` pour lier les concepts entre eux
- Si une note référence un outil (`yq`), crée ou mets à jour la note de cet outil

### Dossiers à utiliser (créer si absents)
| Dossier | Contenu |
|---------|---------|
| `bash/` | Commandes, options, syntax bash |
| `gitlab-ci/` | Jobs, variables, structure pipeline |
| `outils/` | yq, jq, apk, etc. |
| `snippets/` | Blocs de code réutilisables, sans explication longue |

Cette liste n'est pas exhaustive : le vault contient de nombreux autres dossiers de domaine (`azure/`, `docker/`, `angular/`, `dotnet/`, `poo/`, `reseau/`, `tls/`, `IaC/`, `graphql/`, `kubernetes/`, `linux/`, `securite/`, `powershell/`, `windows-iis/`, `cache/`, `git/`, `mcp/`, `observabilite/`, `perso/`…), certains avec des sous-dossiers (ex. `azure/app-service/`, `docker/swarm/`). Avant de créer une note, vérifie toujours (Glob) si un dossier de domaine (ou sous-dossier) pertinent existe déjà plutôt que d'en créer un nouveau.

### Fichiers index (MOC) — un par dossier et sous-dossier
Chaque dossier de domaine et chaque sous-dossier a son propre fichier MOC, **colocalisé dans le dossier lui-même** (pas dans un `_index/` centralisé) et nommé `_index-<nom-du-dossier>.md` — jamais juste `_index.md`, ce nom se répéterait dans chaque dossier et rendrait les liens `[[_index]]` ambigus (Obsidian résout par nom de fichier, pas par chemin).

Exemples : `azure/_index-azure.md` (domaine), `azure/app-service/_index-app-service.md` (sous-dossier), `docker/swarm/_index-swarm.md`.

Pour chaque domaine/sous-dossier touché :
1. Mets à jour (ou crée) son `_index-<nom>.md` : liste les notes du dossier avec une ligne de description et un lien `[[note]]`.
2. Si le dossier a des sous-dossiers, le MOC du dossier parent ne liste PAS les notes des sous-dossiers en double — il pointe vers leur `_index-<sous-dossier>.md` via une section "## Sous-dossiers" (ex. `- [[_index-app-service|App Service]]`).
3. Si un nouveau domaine de premier niveau est créé, ajoute une ligne vers son `_index-<domaine>.md` dans `_index/_accueil.md` (le sommaire global reste seul dans `_index/`, à la racine du vault).

## Processus

1. Lis `~/.claude/obsidian-vault` pour obtenir le chemin du vault
2. Liste les concepts de la conversation qui méritent une note
3. Pour chaque concept, vérifie si une note existe déjà dans le vault (Glob) avant d'en créer une nouvelle
4. Si la note existe : mets-la à jour ou complète-la
5. Si elle n'existe pas : crée-la
6. Mets à jour le fichier MOC du domaine concerné
7. Résume à l'utilisateur ce qui a été créé / mis à jour

## Ce qu'il ne faut PAS noter
- Le contexte conversationnel ou les échanges eux-mêmes
- Des informations propres à un projet spécifique (noms de variables internes, secrets, etc.)
- Des choses déjà documentées dans le code ou dans un README
