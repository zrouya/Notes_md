---
tags: [ssh, securite, mitm]
---

# Host key et known_hosts — authentifier le serveur

Chaque serveur SSH possède une **host key** (paire de clés propre à la machine). Le client mémorise sa clé publique dans `~/.ssh/known_hosts` pour détecter un serveur usurpé.

## Exemple

Première connexion :

```
The authenticity of host 'gitlab.example.com (10.0.0.5)' can't be established.
ED25519 key fingerprint is SHA256:Xk3...q9A.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Commandes utiles :

```bash
ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub   # empreinte (côté serveur)
ssh-keyscan gitlab.example.com >> ~/.ssh/known_hosts # pré-remplir (CI)
ssh-keygen -R gitlab.example.com                     # oublier une entrée
```

## Détails

- Modèle **TOFU** (*Trust On First Use*) : on fait confiance à la première connexion, puis on vérifie à chaque fois. Comparer l'empreinte à une source sûre (doc de GitHub/GitLab, admin) avant de taper `yes`.
- Le serveur prouve qu'il détient la clé privée en **signant le hash d'échange** pendant le handshake, voir [[ssh-connexion-deroule]].
- `WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!` signifie que la clé ne correspond plus. Causes : serveur réinstallé, IP réattribuée… ou attaque **MITM**. À vérifier avant `ssh-keygen -R`.
- Fichiers côté serveur : `/etc/ssh/ssh_host_*_key` (privée) et `.pub`.
- `StrictHostKeyChecking` (dans [[ssh-config]]) : `ask` (défaut), `accept-new` (accepte les nouveaux hôtes, refuse les changements), `no` (à éviter).
- Alternative sans TOFU : des **certificats SSH** signés par une CA interne (`@cert-authority` dans `known_hosts`).

## Voir aussi

- [[ssh-vue-ensemble]]
- [[securite-https-hsts-pinning-mitm]]
- [[autorite-certification]]
