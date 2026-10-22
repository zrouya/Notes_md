---
tags: [ssh, configuration]
---

# ~/.ssh/config — configuration du client SSH

Fichier qui définit des **alias d'hôtes** et leurs options. On tape `ssh prod` au lieu de `ssh -i ~/.ssh/cle -p 2222 admin@10.1.2.3`.

## Exemple

```sshconfig
# Valeurs par défaut pour tous les hôtes
Host *
    AddKeysToAgent yes          # ajoute la clé à l'agent au premier usage
    ServerAliveInterval 60      # keepalive (évite les coupures NAT/firewall)

Host prod
    HostName 10.1.2.3
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_prod
    IdentitiesOnly yes          # n'essaie QUE cette clé

Host cible-interne
    HostName 10.0.5.20
    ProxyJump bastion           # rebond via un bastion

Host gitlab-pro
    HostName gitlab.example.com
    User git
    IdentityFile ~/.ssh/id_gitlab_pro
```

## Détails

- **La première valeur trouvée gagne** : mettre les blocs spécifiques **avant** `Host *`.
- `IdentitiesOnly yes` évite l'erreur `Too many authentication failures`, qui survient quand l'agent propose trop de clés avant la bonne.
- `Host` accepte des jokers (`Host *.interne`).
- Fichier système : `/etc/ssh/ssh_config`, ou `C:\ProgramData\ssh\ssh_config` sous Windows.
- Voir la config effective d'un hôte : `ssh -G prod`.
- Un alias `Host` sert aussi à gérer **plusieurs comptes git**, voir [[git-ssh-plusieurs-comptes]].

## Voir aussi

- [[ssh-agent]]
- [[ssh-tunnels-port-forwarding]]
- [[Bastion]]
