# /obsidian — Alimenter le vault Obsidian

## Étape 0 — Trouver le vault

Lis le fichier `~/.claude/obsidian-vault` (via le tool Read, chemin complet : `C:/Users/zrouya.externe/.claude/obsidian-vault` sur le PC pro, mais utilise toujours Read sur `~/.claude/obsidian-vault` pour être portable).

En pratique : utilise le tool Read sur le chemin `~/.claude/obsidian-vault` — la première ligne contient le chemin absolu du vault à utiliser. Si le fichier est absent, informe l'utilisateur qu'il doit le créer avec `echo "C:\chemin\vers\vault" > ~/.claude/obsidian-vault`.

## Ta mission

Passe en revue l'échange en cours et identifie tout ce qui mérite d'être capitalisé :
commandes shell, concepts techniques, snippets, tips, outils, patterns d'architecture, workflows, etc.

## Règles de génération

### Structure des fichiers
- **Un fichier par concept atomique** : `Record Struct en CSharp.md`, `Composition Root.md`
- **Fichiers courts** : 20-60 lignes max. Si un sujet est large, découpe-le en plusieurs notes liées entre elles.
- **Nommage** : Title Case avec espaces, en français (convention du vault — ex. `Architecture Hexagonale (Ports et Adapters).md`, pas `architecture-hexagonale.md`)

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

- [[Note liée 1]]
- [[Note liée 2]]
```

### Liens Obsidian
- Utilise `[[Nom de la note]]` (bare, sans chemin) pour lier deux notes normales entre elles — Obsidian résout par nom de fichier, peu importe le dossier où elles se trouvent. Déplacer une note ne casse donc jamais ces liens.
- Si une note référence un outil ou concept qui a sa propre note (`yq`, `IEnvelopeStore`...), crée ou mets à jour cette note et lie-la.

## Organisation du vault : dossiers par domaine + MOC à chaque nœud

Le vault est organisé en arborescence : un dossier par grand domaine à la racine, avec sous-dossiers quand un domaine devient volumineux, et un fichier `_index.md` (MOC — Map of Content) **dans chaque dossier et sous-dossier**, y compris à la racine du vault.

### Trouver ou créer le bon dossier
1. Liste les dossiers de premier niveau du vault (`Glob` à la racine) pour voir les domaines déjà existants (ex. `Azure/`, `Réseaux/`, `Docker et Conteneurisation/`, `CSharp et .NET/`, `Angular et TypeScript/`, `Sécurité et Authentification/`, `Shell et Systèmes/`, `Architecture et Design Patterns/`, `Perso/`, `_attachments/`).
2. Si un dossier existant correspond clairement au sujet de la note, utilise-le (et son sous-dossier le plus pertinent si le domaine en a).
3. Sinon, crée un nouveau dossier de domaine à la racine, nommé en Title Case français (ex. `Kubernetes/`, `Terraform/`). Ne force jamais une note dans un domaine qui ne lui correspond pas.
4. Un domaine ne se découpe en sous-dossiers que s'il dépasse ~15-20 notes — sinon les notes restent directement dans le dossier du domaine.
5. Les images/captures d'écran collées (`Pasted image *.png`, captures) vont dans `_attachments/` à la racine du vault, jamais dans un dossier de domaine — les embeds `![[image.png]]` d'Obsidian les résolvent par nom peu importe où elles sont.

### Fichiers `_index.md` (MOC)
Chaque dossier — y compris la racine du vault — a son propre `_index.md` :
```markdown
---
tags: [moc, <domaine>]
---

# <Nom du dossier ou du domaine>

<1-2 phrases décrivant le périmètre>

## Notes

- [[Nom de la note 1]] — courte description (une ligne)
- [[Nom de la note 2]] — courte description

## Sous-dossiers

- [[Domaine/Sous-dossier/_index|Nom du sous-dossier]] — courte description
```

**Règle impérative sur les liens vers un `_index.md`** : comme ce nom de fichier se répète dans CHAQUE dossier du vault, un lien nu `[[_index]]` est ambigu — Obsidian ne peut pas savoir lequel choisir. Un lien vers un index doit TOUJOURS utiliser le chemin relatif complet avec un alias via `|` : `[[Azure/Stockage et Données/_index|Stockage et Données]]`. Ne jamais écrire `[[_index]]` seul.

Le `_index.md` à la racine du vault liste tous les domaines de premier niveau, chacun via ce même format de lien complet.

## Processus

1. Lis `~/.claude/obsidian-vault` pour obtenir le chemin du vault
2. Liste les concepts de la conversation qui méritent une note
3. Pour chaque concept :
   - Vérifie si une note existe déjà dans le vault (Glob, recherche par nom sur tout le vault, pas seulement à la racine) avant d'en créer une nouvelle
   - Identifie le dossier de domaine pertinent (existant ou nouveau, voir ci-dessus)
   - Si la note existe : mets-la à jour ou complète-la, sans la déplacer sauf si son dossier actuel ne correspond plus au sujet
   - Si elle n'existe pas : crée-la dans le bon dossier/sous-dossier
4. Mets à jour le(s) `_index.md` concerné(s) : celui du dossier où la note a été ajoutée, et celui du dossier parent si un nouveau sous-dossier ou domaine a été créé (jusqu'à la racine du vault si un nouveau domaine de premier niveau apparaît)
5. Résume à l'utilisateur ce qui a été créé / mis à jour, et dans quel(s) dossier(s)

## Ce qu'il ne faut PAS noter
- Le contexte conversationnel ou les échanges eux-mêmes
- Des informations propres à un projet spécifique (noms de variables internes, secrets, etc.)
- Des choses déjà documentées dans le code ou dans un README
