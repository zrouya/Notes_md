---
tags: [reseau, securite, firewall]
---

# Filtrage stateful vs stateless

Un filtre réseau **stateful** se souvient des connexions en cours et laisse passer leurs réponses automatiquement. Un filtre **stateless** juge chaque paquet isolément, sans mémoire.

## Pourquoi c'est important : une connexion va dans les deux sens

Quand un client appelle un serveur en HTTPS, des paquets partent **dans les deux sens** :

```
Client 10.0.1.5:51234  ──── SYN ────>  Serveur 10.0.2.8:443      (aller)
Client 10.0.1.5:51234  <── SYN-ACK ──  Serveur 10.0.2.8:443      (retour)
```

Le port côté client (`51234`) est un **port éphémère** tiré au hasard (voir [[Ports réseaux]]).

## Stateless : il faut tout écrire

Il faut deux règles : l'aller vers `443` **et** le retour vers « n'importe quel port éphémère » (`1024-65535`). Cette seconde règle est très large et facile à oublier.

## Stateful : on écrit l'aller, le retour suit

Le filtre voit passer le `SYN`, note la connexion dans une **table d'état** (IP et ports source/destination), puis reconnaît les paquets de réponse et les laisse passer.

→ On n'écrit que la règle « autoriser l'entrée sur 443 ».

## Qui est quoi

| Filtre | Type |
|--------|------|
| [[network-security-group-nsg-azure\|NSG Azure]] | Stateful |
| Azure Firewall, pare-feu d'entreprise | Stateful |
| iptables avec `conntrack` | Stateful |
| Network ACL AWS, ACL de routeur classique | Stateless |

## Conséquence : le retour doit repasser par le même filtre

Un filtre stateful n'accepte une réponse que s'il a **vu l'aller**. Si l'aller passe par un firewall et le retour par un autre chemin, le firewall jette les paquets. C'est le [[routage-asymetrique]].

## Voir aussi

- [[Firewall]] · [[Cession TCP]]
- [[ports-ephemeres-time-wait]]
