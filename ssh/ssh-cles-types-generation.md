---
tags: [ssh, cryptographie, ssh-keygen]
---

# Clés SSH — types, fichiers, ssh-keygen

`ssh-keygen` crée et gère les paires de clés. Le type recommandé aujourd'hui est **Ed25519**.

## Syntaxe / Exemple

```bash
ssh-keygen -t ed25519 -C "prenom.nom@exemple.fr"         # nouvelle clé
ssh-keygen -t ed25519 -f ~/.ssh/id_gitlab_pro            # nom de fichier choisi
ssh-keygen -p -f ~/.ssh/id_ed25519                       # changer la passphrase
ssh-keygen -lf ~/.ssh/id_ed25519.pub                     # empreinte SHA256
ssh-keygen -y -f ~/.ssh/id_ed25519 > id_ed25519.pub      # recalculer la .pub
```

## Types

| Type | Statut |
|------|--------|
| `ed25519` | **Recommandé** : courte, rapide, sûre |
| `ecdsa` | OK, moins apprécié (courbes NIST) |
| `rsa` | OK en 3072/4096 bits, pour les vieux serveurs |
| `dsa` | Obsolète, désactivé par OpenSSH |
| `ed25519-sk` / `ecdsa-sk` | Clé matérielle FIDO2 (YubiKey) : la clé privée reste dans le token |

## Fichiers

| Fichier | Rôle |
|---------|------|
| `~/.ssh/id_ed25519` | Clé **privée**, à ne jamais partager |
| `~/.ssh/id_ed25519.pub` | Clé **publique**, à déposer sur les serveurs |
| `~/.ssh/known_hosts` | Host keys des serveurs connus |
| `~/.ssh/authorized_keys` | (côté serveur) clés autorisées |
| `~/.ssh/config` | Config client, voir [[ssh-config]] |

Sous Windows, `~` correspond à `C:\Users\<user>`.

## Bonnes pratiques

- Toujours mettre une **passphrase**, et utiliser [[ssh-agent]] pour ne pas la retaper.
- Une clé par poste plutôt qu'une clé copiée partout : on révoque un poste sans toucher aux autres.

## Voir aussi

- [[ssh-authentification-cle-publique]]
- [[ssh-agent]]
