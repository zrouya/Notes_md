---
tags: [ssh, ssh-agent, windows, git]
---

# ssh-agent sous Windows (+ git)

Windows 10/11 embarque OpenSSH et un service `ssh-agent` **persistant** : les clés ajoutées survivent au redémarrage. Il faut aussi que git utilise **ce** ssh, et non celui fourni par Git for Windows.

## Mise en place

```powershell
# 1. Service en démarrage auto (PowerShell ADMIN)
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent

# 2. Charger la clé (une seule fois, passphrase demandée)
ssh-add $env:USERPROFILE\.ssh\id_ed25519

# 3. Faire utiliser à git le ssh de Windows
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"

# 4. Vérifier
ssh-add -l
ssh -T git@github.com
```

## Le piège : deux ssh.exe

| Binaire | Agent utilisé |
|---------|---------------|
| `C:\Windows\System32\OpenSSH\ssh.exe` | Service Windows (named pipe) |
| `C:\Program Files\Git\usr\bin\ssh.exe` | Agent de Git Bash (`SSH_AUTH_SOCK`) |

Par défaut git prend le second, qui ne voit pas les clés du service Windows : la passphrase est redemandée à chaque fois. `core.sshCommand` (ou la variable d'environnement `GIT_SSH`) corrige ça.

## Alternative sans droits admin (Git Bash seul)

Dans `~/.bashrc` :

```bash
eval "$(ssh-agent -s)" > /dev/null
ssh-add ~/.ssh/id_ed25519 2>/dev/null
```

La passphrase est alors redemandée à chaque nouveau terminal, et ça ne marche que dans Git Bash.

## Diagnostic

- Un **mot de passe** est demandé (et non une passphrase) : le remote est en HTTPS, voir [[git-ssh-vs-https]].
- `Error connecting to agent` : le service est arrêté (`Get-Service ssh-agent`).

## Voir aussi

- [[ssh-agent]]
- [[git-ssh-fonctionnement]]
