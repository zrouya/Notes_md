---
tags: [ssh, reseau, tunnel]
---

# Tunnels SSH — redirection de ports

La couche *connection* de SSH peut transporter n'importe quel flux TCP dans un **canal chiffré**. Cela permet d'atteindre un service qui n'est pas exposé directement.

## Syntaxe / Exemple

```bash
# -L : local → distant. localhost:5433 (poste) ⇒ db.interne:5432 vu depuis le serveur
ssh -L 5433:db.interne:5432 user@bastion
psql -h localhost -p 5433

# -R : distant → local. serveur:8080 ⇒ localhost:3000 du poste
ssh -R 8080:localhost:3000 user@serveur

# -D : proxy SOCKS dynamique sur localhost:1080
ssh -D 1080 user@serveur

# -J : rebond (ProxyJump) via un bastion
ssh -J user@bastion user@cible

# -N : pas de shell, tunnel seulement ; -f : en arrière-plan
ssh -fN -L 5433:db.interne:5432 user@bastion
```

## Schéma `-L`

```
appli ─► localhost:5433 ═══ tunnel SSH chiffré ═══► sshd (bastion) ─► db.interne:5432
         (poste)                                    (en clair, sur le réseau interne)
```

## Détails

- Le segment `sshd → destination` n'est **pas** chiffré par SSH : il circule sur le réseau du serveur.
- Côté serveur, `AllowTcpForwarding` et `GatewayPorts` (dans `sshd_config`) contrôlent ce qui est permis.
- Chaque tunnel est un **canal** distinct multiplexé sur la même connexion TCP, voir [[ssh-architecture-protocole]].

## Voir aussi

- [[ssh-config]]
- [[Bastion]]
- [[Virtual Private Network (VPN)]]
