---
tags: [reseau, tcp, handshake]
---

# Session TCP — établissement, états, fermeture

> Note : le titre historique dit « Cession », il s'agit bien d'une **session** TCP.

## Ouverture — handshake en 3 temps

```
→ SYN       le client propose son n° de sequence initial
← SYN-ACK   le serveur accuse reception et propose le sien
→ ACK       le client accuse reception     -> ETABLIE
```

Coût : **1 RTT** avant le premier octet applicatif. Incompressible en TCP — c'est exactement ce que QUIC élimine en fusionnant connexion et TLS ([[http-versions]]).

## Les réponses possibles à un SYN

| Réponse | Signification | Erreur vue par l'appelant |
|---------|---------------|---------------------------|
| `SYN-ACK` | Un service écoute | — connexion établie |
| `RST` | La machine répond, **rien n'écoute** | `ECONNREFUSED` |
| *rien* | Paquets jetés en silence | `ETIMEDOUT` |

Cette table est la base du diagnostic : voir [[erreurs-connexion-econnrefused]].

## États principaux

| État | Sens |
|------|------|
| `LISTEN` | Le service attend des connexions |
| `SYN_SENT` | SYN envoyé, en attente de réponse |
| `ESTABLISHED` | Connexion active |
| `FIN_WAIT` / `CLOSE_WAIT` | Fermeture en cours |
| `TIME_WAIT` | Quadruplet réservé ~2 min après fermeture |

```bash
ss -tan                    # etats de toutes les connexions TCP
ss -tan state established
```

## Fermeture

Symétrique : `FIN` de chaque côté, chacun accusé. Le côté qui ferme **en premier** passe en [[ports-ephemeres-time-wait|TIME_WAIT]] — origine des épuisements de ports sous charge.

## Voir aussi

- [[Protocole TCP]]
- [[cout-rtt-connexion-https]]
- [[Ports réseaux]]
