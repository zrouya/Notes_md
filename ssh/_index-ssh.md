---
tags: [index, moc, ssh]
---

# Index — SSH

Secure Shell : protocole, flux, clés, agent, configuration client et tunnels.

## Fondamentaux

| Note | Description |
|------|-------------|
| [[ssh-vue-ensemble]] | Ce qu'est SSH, ses 3 garanties, ses usages, différence avec TLS |
| [[ssh-architecture-protocole]] | Les 3 couches : transport, user auth, connection (canaux) |
| [[ssh-connexion-deroule]] | Flux complet : versions, KEXINIT, ECDH, NEWKEYS, auth, canal |

## Authentification

| Note | Description |
|------|-------------|
| [[ssh-host-key-known-hosts]] | Authentifier le serveur : TOFU, empreinte, alerte MITM |
| [[ssh-authentification-cle-publique]] | Authentifier le client : signature du session ID, `authorized_keys` |
| [[ssh-cles-types-generation]] | Ed25519, RSA, FIDO2 ; `ssh-keygen` ; fichiers de `~/.ssh` |
| [[ssh-agent]] | Garder les clés déchiffrées en mémoire ; agent forwarding |
| [[ssh-agent-windows]] | Service Windows + `core.sshCommand` : le piège des deux `ssh.exe` |

## Usage avancé

| Note | Description |
|------|-------------|
| [[ssh-config]] | Alias `Host`, `IdentityFile`, `IdentitiesOnly`, `ProxyJump` |
| [[ssh-tunnels-port-forwarding]] | `-L`, `-R`, `-D`, `-J` |

## Git sur SSH

| Note | Description |
|------|-------------|
| [[git-ssh-fonctionnement]] | `git-upload-pack` / `git-receive-pack`, compte `git@` partagé |
| [[git-ssh-vs-https]] | Reconnaître le transport d'un remote, en changer, diagnostic |
| [[git-ssh-plusieurs-comptes]] | Deux comptes sur la même forge via des alias `Host` |

## Liens transverses

- [[Secure shell (SSH)]] : fiche protocole dans le corpus réseau
- [[_index-tls\|TLS / HTTPS]] : l'autre grand protocole de canal chiffré
- [[_index-reseau\|Réseau]]
