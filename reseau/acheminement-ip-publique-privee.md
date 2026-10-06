---
tags: [reseau, ip, routage, dns, arp, bgp, nat]
---

# Comment une IP publique ou privée est atteinte

Joindre une adresse se fait en **trois questions successives** : quel nom → quelle IP (DNS), quel chemin → quel prochain saut (routage), quelle carte réseau → quelle MAC (ARP). Le type d'IP change surtout la deuxième question.

## 1. Nom → IP : le DNS

- **Public** : résolution récursive depuis la racine, visible de partout — voir [[Domain Name System (DNS)]].
- **Privé** : zone DNS interne (AD, Private DNS Zone Azure, `/etc/hosts`) visible seulement depuis le réseau qui l'interroge.
- **Split-horizon** : un même nom répond une IP privée en interne et une IP publique depuis Internet. C'est le cas typique d'un private endpoint.

Le DNS ne fait que **traduire** : il ne garantit pas que l'IP renvoyée soit joignable.

## 2. IP → chemin : le routage

La machine compare l'IP de destination à sa table de routage (**plus long préfixe** gagne) :

```
destination dans mon subnet ?  ── oui ──▶ livraison directe (ARP)
          │ non
          ▼
route spécifique ? (VPN, peering, UDR) ── oui ──▶ ce next-hop
          │ non
          ▼
route par défaut 0.0.0.0/0 ──▶ passerelle ──▶ (NAT) ──▶ Internet
```

| | IP privée | IP publique |
|---|---|---|
| Qui connaît la route | Les routeurs du **domaine privé** (LAN, VNet, peerings, VPN/ExpressRoute) | **Tout Internet**, via BGP |
| Entre réseaux | Uniquement si une route est explicitement établie | Saut d'AS en AS (opérateurs) jusqu'à l'AS propriétaire |
| Sans route | Paquet jeté (ou envoyé vers la route par défaut et filtré) | — |

**BGP** : chaque propriétaire de bloc public (opérateur, Microsoft, AWS…) **annonce** ses préfixes à ses voisins ; ces annonces se propagent et construisent la carte d'Internet.

## 3. IP → MAC : le dernier saut

Sur le lien local, l'IP ne suffit pas : [[Adress Resolution Protocol (ARP)|ARP]] (IPv4) ou NDP (IPv6) trouve la **MAC** de la destination ou de la passerelle. Pour une IP hors subnet, on n'ARP **jamais** la destination finale, seulement la passerelle.

## Diagnostiquer

```bash
nslookup app.exemple.com        # quelle IP le DNS renvoie (privée ou publique ?)
ip route get 10.0.1.4           # Linux : quelle route/interface sera utilisée
Find-NetRoute -RemoteIPAddress 10.0.1.4   # Windows
tracert / traceroute 20.50.1.1  # par où ça passe
```

## Voir aussi

- [[ip-publique-vs-privee]] · [[plages-ip-reservees]]
- [[Routage réseau]] · [[Passerelle réseau]]
- [[NAT (Network Address Translation)]]
- [[private-dns-zone]] · [[diagnostic-par-couche]]
