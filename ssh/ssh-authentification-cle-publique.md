---
tags: [ssh, authentification, cryptographie]
---

# Authentification SSH par clé publique

Le client prouve qu'il détient une **clé privée** sans jamais l'envoyer : il **signe** une donnée propre à la session, et le serveur vérifie la signature avec la clé publique enregistrée.

## Mise en place

```bash
ssh-keygen -t ed25519 -C "prenom.nom@exemple.fr"   # génère la paire
ssh-copy-id user@serveur                            # ajoute la .pub côté serveur
# équivalent manuel : coller ~/.ssh/id_ed25519.pub dans
#   ~/.ssh/authorized_keys du compte distant
```

Pour GitHub/GitLab/Azure DevOps, coller la `.pub` dans *Settings > SSH keys*.

## Flux

```
Client                                        Serveur
  │── USERAUTH_REQUEST publickey (clé pub, ─────►│  "cette clé est-elle acceptée ?"
  │   sans signature)                            │  cherche dans authorized_keys
  │◄─ USERAUTH_PK_OK ────────────────────────────│
  │── USERAUTH_REQUEST publickey + signature ───►│  signature de (session ID +
  │                                              │  user + clé) par la clé privée
  │◄─ USERAUTH_SUCCESS ──────────────────────────│  vérifiée avec la clé publique
```

## Pourquoi c'est mieux qu'un mot de passe

- Le secret (clé privée) **ne quitte jamais** le poste client.
- La signature porte sur le **session ID**, unique à la connexion : impossible de la rejouer ailleurs.
- Un serveur compromis ne récupère qu'une clé publique, sans valeur pour un attaquant.

## Détails

- `authorized_keys` : une clé par ligne, avec des options possibles (`command="..."`, `from="10.0.0.0/8"`, `no-port-forwarding`).
- Droits stricts exigés : `~/.ssh` en 700, `authorized_keys` en 600, sinon `sshd` ignore le fichier.
- La **passphrase** chiffre la clé privée **sur le disque**. Elle ne transite jamais et sert seulement à déchiffrer la clé en local, voir [[ssh-agent]].

## Voir aussi

- [[ssh-cles-types-generation]]
- [[ssh-connexion-deroule]]
- [[authentification]]
