---
tags: [ssh, ssh-agent, outils]
---

# ssh-agent — ne taper la passphrase qu'une fois

Processus local qui garde les clés privées **déchiffrées en mémoire** et réalise les signatures à la place du client `ssh`. La passphrase n'est demandée qu'au `ssh-add`.

## Syntaxe / Exemple

```bash
eval "$(ssh-agent -s)"           # démarrer un agent (Linux / Git Bash)
ssh-add ~/.ssh/id_ed25519        # charger une clé (demande la passphrase)
ssh-add -l                       # lister les clés chargées
ssh-add -D                       # tout décharger
ssh-add -t 8h ~/.ssh/id_ed25519  # clé valable 8 h seulement
```

## Fonctionnement

```
ssh ──(socket / named pipe)──► ssh-agent ──► signe avec la clé privée
 │                                 ▲
 └─ demande "signe ce session ID" ─┘   la clé ne sort jamais de l'agent
```

- Linux et macOS : socket Unix désigné par la variable `SSH_AUTH_SOCK`.
- Windows : service `ssh-agent` + named pipe `\\.\pipe\openssh-ssh-agent`, voir [[ssh-agent-windows]].
- macOS : `UseKeychain yes` dans [[ssh-config]] pour stocker la passphrase dans le trousseau.

## Agent forwarding (`ssh -A`)

Permet d'utiliser son agent local depuis un serveur distant (par exemple un `git pull` sur un bastion avec sa propre clé).

- **Risque** : root sur ce serveur peut se servir de l'agent tant que la session est ouverte.
- Préférer `ProxyJump` (`ssh -J bastion cible`), qui ne délègue rien.

## Voir aussi

- [[ssh-agent-windows]]
- [[ssh-cles-types-generation]]
- [[ssh-config]]
