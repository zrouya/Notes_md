---
tags: [ssh, protocole, reseau]
---

# SSH — architecture en 3 couches

SSH-2 est découpé en trois sous-protocoles empilés au-dessus de TCP (RFC 4251 à 4254).

## Schéma

```
┌───────────────────────────────────────────────┐
│ Connection Protocol (RFC 4254)                │  canaux : shell, exec,
│   canal 0 : session   canal 1 : tunnel -L ... │  subsystem sftp, forwarding
├───────────────────────────────────────────────┤
│ User Authentication Protocol (RFC 4252)       │  publickey, password,
│                                               │  keyboard-interactive
├───────────────────────────────────────────────┤
│ Transport Layer Protocol (RFC 4253)           │  échange de clés, chiffrement,
│                                               │  intégrité, auth du serveur
├───────────────────────────────────────────────┤
│ TCP (port 22)                                 │
└───────────────────────────────────────────────┘
```

## Rôle de chaque couche

- **Transport** : négocie les algorithmes, fait l'échange de clés (ECDH), prouve l'identité du **serveur** avec sa host key, puis chiffre tous les paquets. Rien ne passe en clair après `NEWKEYS`.
- **User auth** : le **client** prouve qui il est, déjà à l'intérieur du tunnel chiffré. Le mot de passe éventuel n'est donc jamais en clair sur le réseau.
- **Connection** : **multiplexe** plusieurs *canaux* logiques sur une seule connexion TCP (un shell, un `scp`, un tunnel…). Chaque canal a son propre contrôle de flux (*window size*).

## Format d'un paquet (couche transport)

```
packet_length | padding_length | payload | padding aléatoire | MAC
```

Le payload commence par un **numéro de message** (`SSH_MSG_KEXINIT` = 20, `SSH_MSG_USERAUTH_REQUEST` = 50, `SSH_MSG_CHANNEL_DATA` = 94…).

## Voir aussi

- [[ssh-connexion-deroule]]
- [[ssh-vue-ensemble]]
- [[Modèle TCP_IP]]
