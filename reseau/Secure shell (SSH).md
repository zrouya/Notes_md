---
tags: [reseau, ssh, protocole]
---

# Secure Shell (SSH)

Protocole de la couche application (modèle TCP/IP) qui fournit un **accès distant chiffré et authentifié**, au-dessus de [[Protocole TCP|TCP]], sur le **port 22**.

## En bref

- Remplace Telnet, rsh et rlogin, qui faisaient tout passer en clair.
- Le serveur est authentifié par sa *host key* et le client par mot de passe ou clé.
- Transporte des shells, des commandes, des fichiers (`scp`, `sftp`), git et des tunnels TCP.

## Notes détaillées

Le sujet est développé dans le dossier `ssh/` : [[_index-ssh|Index — SSH]].

- [[ssh-vue-ensemble]]
- [[ssh-connexion-deroule]]
- [[ssh-tunnels-port-forwarding]]

## Voir aussi

- [[Ports réseaux]]
- [[Remote Desktop Protocol (RDP)]]
- [[Bastion]]
