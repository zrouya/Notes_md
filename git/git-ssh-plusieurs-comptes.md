---
tags: [git, ssh, configuration]
---

# Plusieurs comptes git sur la même forge (SSH)

Une forge identifie le compte par la **clé**. Pour utiliser deux comptes (perso et pro) sur le même hôte, on crée deux alias dans `~/.ssh/config`, chacun avec sa clé.

## Configuration

```sshconfig
Host github-perso
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_perso
    IdentitiesOnly yes

Host github-pro
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_pro
    IdentitiesOnly yes
```

## Utilisation

```bash
git clone git@github-pro:entreprise/projet.git
git remote set-url origin git@github-perso:moi/projet-perso.git
ssh -T git@github-pro          # vérifie quel compte répond
```

## Détails

- **`IdentitiesOnly yes` est indispensable** : sans lui, l'agent propose d'abord la première clé chargée, et la forge t'identifie sur le mauvais compte.
- Ponctuellement, sans alias : `GIT_SSH_COMMAND="ssh -i ~/.ssh/id_pro -o IdentitiesOnly=yes" git fetch`.
- Penser aussi à l'identité des **commits** par dépôt (`git config user.email ...`), indépendante de la clé. Ou avec `includeIf "gitdir:~/pro/"` dans `~/.gitconfig`.

## Voir aussi

- [[ssh-config]]
- [[git-ssh-fonctionnement]]
