---
tags: [git, ssh, remote]
---

# Git sur SSH — comment ça marche

Avec un remote `git@serveur:groupe/projet.git`, git **lance le client `ssh`** et fait exécuter côté serveur un petit programme git. Le protocole git circule ensuite dans le canal SSH, sur ses stdin/stdout.

## Ce que git exécute réellement

```bash
# git fetch / pull / clone
ssh git@gitlab.example.com "git-upload-pack 'groupe/projet.git'"
# git push
ssh git@gitlab.example.com "git-receive-pack 'groupe/projet.git'"
```

Visible avec `GIT_SSH_COMMAND="ssh -v" git fetch` ou `GIT_TRACE=1 git fetch`.

## Flux d'un `git push`

```
git push
  │ 1. lance ssh (ou core.sshCommand) vers git@serveur
  │ 2. handshake SSH, host key vérifiée (known_hosts)
  │ 3. auth par clé : l'agent signe (passphrase déjà saisie)
  │ 4. exec "git-receive-pack 'groupe/projet.git'"
  │ 5. le serveur annonce ses refs (branches + SHA)
  │ 6. le client calcule les objets manquants et envoie un packfile
  │ 7. le serveur met à jour les refs (hooks, protections de branche)
  ▼ "To gitlab.example.com:groupe/projet.git  abc123..def456  main -> main"
```

## Pourquoi tout le monde se connecte en `git@`

- Tous les utilisateurs partagent le **même compte système** `git`.
- L'**identité** vient de la **clé** : le serveur cherche quelle clé publique a signé et la rattache à ton compte GitHub/GitLab/Azure DevOps.
- `authorized_keys` force une commande (`command="gitlab-shell key-42"`) : impossible d'obtenir un vrai shell.
- `ssh -T git@github.com` répond `Hi <user>! You've successfully authenticated...` : c'est la manière de vérifier quelle identité la clé porte.

## Points notables

- L'auteur des commits (`user.name` / `user.email`) n'a **aucun lien** avec la clé SSH : la clé sert à l'accès, pas à la signature des commits.
- Pour signer ses commits avec une clé SSH : `git config gpg.format ssh` et `user.signingkey ~/.ssh/id_ed25519.pub`.

## Voir aussi

- [[git-ssh-vs-https]]
- [[git-ssh-plusieurs-comptes]]
- [[ssh-agent-windows]]
- [[ssh-authentification-cle-publique]]
