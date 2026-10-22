---
tags: [git, ssh, https, remote]
---

# Remote git : SSH ou HTTPS

Le format de l'URL du remote détermine le transport, et donc le type d'authentification demandé.

## Reconnaître et changer

```bash
git remote -v
# origin  git@gitlab.example.com:groupe/projet.git        → SSH (forme scp)
# origin  ssh://git@gitlab.example.com:2222/groupe/projet → SSH (port explicite)
# origin  https://gitlab.example.com/groupe/projet.git    → HTTPS

git remote set-url origin git@gitlab.example.com:groupe/projet.git
```

Réécriture globale (toutes les URLs HTTPS d'un hôte passent en SSH) :

```bash
git config --global url."git@gitlab.example.com:".insteadOf "https://gitlab.example.com/"
```

## Comparaison

| | SSH | HTTPS |
|---|---|---|
| Port | 22 (parfois 443 via `ssh.github.com`) | 443 |
| Auth | Paire de clés | Login + **PAT** (token) ou OAuth |
| Saisie répétée évitée par | [[ssh-agent]] | Credential Manager (`credential.helper manager`) |
| Proxy d'entreprise | Souvent **bloqué** | Passe généralement |
| Ce qui est demandé | *Enter passphrase for key...* | *Username / Password* |

## Diagnostic rapide

- `Password:` ou `Username for 'https://...'` : remote HTTPS, l'agent SSH n'y peut rien.
- `Permission denied (publickey)` : SSH, mais aucune clé acceptée. Vérifier `ssh -T git@hote`, `ssh-add -l` et la clé déposée sur la forge.
- `Connection timed out` sur le port 22 : firewall, essayer HTTPS ou SSH sur 443.

## Voir aussi

- [[git-ssh-fonctionnement]]
- [[ssh-agent-windows]]
- [[HTTP(S)]]
