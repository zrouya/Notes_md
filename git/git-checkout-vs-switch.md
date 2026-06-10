---
tags: [git, branche, switch, checkout, restore]
---

# checkout vs switch — pourquoi la scission

`git checkout` est une commande **historique surchargée** (≥ 4 rôles distincts). Git 2.23 (2019) l'a scindée en [[git-switch]] (branches) + [[git-restore]] (fichiers) pour lever l'ambiguïté et les pièges destructeurs.

## Les 4 casquettes de checkout

```bash
git checkout main              # 1. changer de branche
git checkout -b feature/x      # 2. créer + basculer
git checkout v1.2.0            # 3. detached HEAD sur commit/tag
git checkout -- fichier.txt    # 4. ⚠️ restaurer/écraser un fichier
```

## Le piège du n°4

`git checkout -- fichier` **écrase les modifs locales sans confirmation** ni récupération (jamais commité/stashé → invisible au reflog). Ambiguïté en prime : si une branche et un fichier portent le même nom, `git checkout nom` est ambigu → d'où le `--` pour forcer « ceci est un chemin ».

## Ce que change la scission

- **1 commande = 1 responsabilité** : branches ↔ `switch`, fichiers ↔ `restore`.
- **Moins d'ambiguïté** : plus de conflit nom-branche / nom-fichier.
- **Moins de destructif** : `switch` protège les modifs non commitées par défaut ; l'écrasement de fichiers est cantonné à `restore` (intention explicite).
- Différence **ergonomique/sécuritaire, pas fonctionnelle** : `switch`/`restore` ne font rien de neuf, ils clarifient.

`checkout` reste valide (legacy) — surtout utile pour lire d'anciens scripts/tutoriels.

## Tableau de correspondance

| Intention | Ancien | Nouveau |
|-----------|--------|---------|
| Changer de branche | `git checkout main` | `git switch main` |
| Créer + basculer | `git checkout -b x` | `git switch -c x` |
| Créer/réinit + basculer | `git checkout -B x` | `git switch -C x` |
| Détacher sur commit | `git checkout <sha>` | `git switch -d <sha>` |
| Rejeter modifs fichier | `git checkout -- f` | `git restore f` |
| Restaurer depuis commit | `git checkout <sha> -- f` | `git restore --source=<sha> f` |
| Dé-stager un fichier | `git reset HEAD f` | `git restore --staged f` |

## Voir aussi

- [[git-switch]]
- [[git-restore]]
- [[git-switch-create-branch]]
