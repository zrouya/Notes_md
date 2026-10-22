---
tags: [ssh, reseau, securite]
---

# SSH — vue d'ensemble

**Secure Shell** : protocole applicatif qui ouvre un **canal chiffré et authentifié** entre un client et un serveur, au-dessus de TCP (port **22** par défaut). Il remplace Telnet, rsh et rlogin, qui faisaient tout transiter en clair.

## Syntaxe / Exemple

```bash
ssh user@serveur                 # shell distant
ssh user@serveur "df -h"         # une commande, puis fermeture
scp fichier.txt user@serveur:/tmp/
ssh -T git@github.com            # test d'authentification git
```

## Les 3 garanties

| Garantie | Comment |
|----------|---------|
| **Confidentialité** | Chiffrement symétrique (AES-GCM, ChaCha20-Poly1305) |
| **Intégrité** | MAC ou chiffrement authentifié (AEAD) sur chaque paquet |
| **Authentification** | Du **serveur** par sa host key, du **client** par mot de passe ou clé |

## Usages

- Shell distant (administration Linux)
- Transfert de fichiers : `scp`, `sftp`, `rsync -e ssh`
- **Git** sur un remote `git@...`, voir [[git-ssh-fonctionnement]]
- Tunnels et redirection de ports, voir [[ssh-tunnels-port-forwarding]]

## Différence avec TLS

Même idée de [[cryptographie-hybride]] (asymétrique pour négocier, symétrique pour les données), mais **pas de CA** par défaut : la confiance dans le serveur repose sur `known_hosts` (TOFU), voir [[ssh-host-key-known-hosts]].

## Voir aussi

- [[ssh-architecture-protocole]]
- [[ssh-connexion-deroule]]
- [[Secure shell (SSH)]]
- [[_index-ssh]]
