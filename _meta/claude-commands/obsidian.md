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
| `_index/` | Fichiers MOC (Map of Content) par domaine |

Cette liste n'est pas exhaustive : le vault contient de nombreux autres dossiers de domaine (`azure/`, `docker/`, `angular/`, `dotnet/`, `poo/`, `reseau/`, `tls/`, `IaC/`, `graphql/`, `kubernetes/`, `linux/`, `securite/`, `powershell/`, `windows-iis/`, `cache/`, `git/`, `mcp/`, `observabilite/`, `perso/`…). Avant de créer une note, vérifie toujours (Glob) si un dossier de domaine pertinent existe déjà plutôt que d'en créer un nouveau.

### Fichiers index (MOC)
Pour chaque domaine touché, mets à jour (ou crée) un fichier `_index/nom-domaine.md` qui liste les notes du domaine avec une ligne de description et un lien `[[note]]`. N'oublie pas d'ajouter le domaine à `_index/_accueil.md` s'il est nouveau.

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
