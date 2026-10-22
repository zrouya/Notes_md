---
tags: [ssh, protocole, handshake, reseau]
---

# Connexion SSH — déroulé des flux

Ce qui se passe entre `ssh user@serveur` et l'apparition du prompt.

## Séquence

```
Client                                         Serveur
  │──── TCP SYN / SYN-ACK / ACK (port 22) ────────│  1. connexion TCP
  │◄─── "SSH-2.0-OpenSSH_9.6" ────────────────────│  2. échange de versions
  │──── "SSH-2.0-OpenSSH_for_Windows_9.5" ───────►│     (en clair)
  │◄──► SSH_MSG_KEXINIT (listes d'algos) ◄───────►│  3. négociation (en clair)
  │──── KEX_ECDH_INIT (clé publique éphémère) ───►│  4. échange de clés
  │◄─── KEX_ECDH_REPLY ───────────────────────────│
  │     (clé éph. serveur + host key + signature) │
  │  vérif. host key dans known_hosts              │  5. auth du serveur
  │◄──► SSH_MSG_NEWKEYS ◄────────────────────────►│  6. bascule en chiffré
  ═════════════ tout est chiffré à partir d'ici ══════════════
  │──── SERVICE_REQUEST "ssh-userauth" ──────────►│
  │──── USERAUTH_REQUEST (publickey, signature) ─►│  7. auth du client
  │◄─── USERAUTH_SUCCESS ─────────────────────────│
  │──── CHANNEL_OPEN "session" ──────────────────►│  8. ouverture d'un canal
  │──── CHANNEL_REQUEST "pty-req", "shell" ──────►│
  │◄──► CHANNEL_DATA ... ◄───────────────────────►│  9. données (frappes, sortie)
```

## Points clés

- **Étape 3** : chacun envoie ses algos par ordre de préférence. Le premier de la liste client supporté par le serveur l'emporte (kex, host key, chiffrement, MAC, compression).
- **Étape 4** : Diffie-Hellman sur courbe elliptique (`curve25519-sha256`, ou hybride post-quantique `sntrup761x25519` / `mlkem768x25519` dans les OpenSSH récents). Le secret partagé `K` n'est **jamais transmis**, voir [[diffie-hellman-ephemere]].
- **Hash d'échange `H`** : condensé de tout le handshake (versions, KEXINIT, clés éphémères, `K`). Le serveur **signe `H`** avec sa host key privée. `H` devient le **session ID**.
- **Étape 6** : les clés de chiffrement et de MAC (une par sens) sont dérivées de `K` et `H`.
- **Étape 7** : le client signe le session ID avec sa clé privée, voir [[ssh-authentification-cle-publique]].
- **Re-keying** : l'échange de clés est rejoué périodiquement (volume ou durée) sur une connexion longue.

## Voir pour de vrai

```bash
ssh -v user@serveur     # -vv / -vvv pour plus de détail
```

Les lignes `kex: algorithm:`, `Server host key:`, `Authentications that can continue:` et `Authenticated to` correspondent aux étapes ci-dessus.

## Voir aussi

- [[ssh-architecture-protocole]]
- [[ssh-host-key-known-hosts]]
- [[handshake-tls-1-3]] (comparaison)
