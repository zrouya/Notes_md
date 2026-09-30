---
tags: [azure, reseau, vnet, securite, nsg]
---

# Network Security Group (NSG) Azure

Un NSG est une **liste de règles "autoriser / refuser"** qui filtre le trafic entrant et sortant d'un subnet ou d'une carte réseau. C'est le [[Firewall]] de base d'Azure : il travaille aux couches 3 et 4 (IP et ports), sans lire le contenu HTTP.

## Anatomie d'une règle

```
Priorité  Nom               Source           Port src  Destination     Port dst  Proto  Action
100       Allow-HTTPS-In    Internet         *         10.10.1.0/24    443       TCP    Allow
200       Allow-SQL-App     10.10.1.0/24     *         10.10.2.0/24    1433      TCP    Allow
4000      Deny-All-In       *                *         *               *         *      Deny
```

- **Priorité** de 100 à 4096 : les règles sont lues du **plus petit numéro au plus grand**, et **la première qui correspond s'applique**. Les suivantes sont ignorées.
- Le **port source** vaut presque toujours `*` : côté client, c'est un port éphémère aléatoire (voir [[Ports réseaux]]).

## Stateful : on n'écrit que l'aller

Le NSG est **stateful** : il se souvient des connexions. Si on autorise l'entrée sur le 443, **la réponse repart automatiquement**, sans règle sortante à écrire. Voir [[filtrage-stateful-stateless]].

## Les règles par défaut (non supprimables, priorité 65000+)

| Sens | Règle | Effet |
|------|-------|-------|
| Entrant | `AllowVnetInBound` | Tout le trafic interne est autorisé |
| Entrant | `AllowAzureLoadBalancerInBound` | Sondes de santé Azure |
| Entrant | `DenyAllInBound` | Tout le reste est refusé, dont Internet |
| Sortant | `AllowVnetOutBound` | Interne autorisé |
| Sortant | `AllowInternetOutBound` | Sortie Internet autorisée |
| Sortant | `DenyAllOutBound` | Le reste est refusé |

⚠️ Le tag `VirtualNetwork` couvre **aussi les VNets appairés et l'on-prem connecté**. `AllowVnetInBound` ouvre donc plus large qu'on ne le croit.

## Où l'attacher

- Sur le **subnet** (recommandé, plus simple à maintenir) et/ou sur la **carte réseau** (NIC).
- Si les deux existent, le paquet doit passer **les deux**. En entrée : subnet puis NIC ; en sortie : NIC puis subnet.

## Refus = silence

Un NSG **jette** le paquet sans répondre (`DROP`). Côté client, on obtient donc un **timeout** et non un « connection refused ». Voir [[Firewall#DROP ou REJECT — la distinction qui se voit côté client]].

## Diagnostic

Network Watcher → **IP flow verify** (quelle règle bloque ?) et **Effective security rules** sur la NIC.

## Voir aussi

- [[nsg-service-tags-asg]] : écrire des règles sans lister d'IP
- [[vnet-routage-udr]] : le chemin, avant le filtrage
- [[erreurs-connexion-econnrefused]]
