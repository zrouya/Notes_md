---
tags: [azure, reseau, vnet, dns, hybride, hub-and-spoke]
---

# DNS hybride : articuler le DNS Azure et le DNS d'entreprise

Dans une architecture [[hub-and-spoke]] reliée à l'on-prem, plusieurs résolveurs coexistent. Chaque nom doit finir chez le serveur qui fait autorité pour lui, **quel que soit l'endroit d'où part la question**.

> [!abstract] Portée : spécifique Azure
> Concepts généraux : [[dns-roles-resolveur-autorite]], [[dns-redirection-conditionnelle]], [[dns-split-horizon]].

## Les acteurs

| Acteur | Fait autorité pour | Joignable depuis |
|--------|--------------------|------------------|
| DNS d'entreprise (AD…) | Zones de l'entreprise | On-prem et Azure, s'il est routé |
| `168.63.129.16` | Zones privées liées au VNet, noms Azure | Azure uniquement |
| [[dns-private-resolver-azure\|Private Resolver]] | Rien : il relaie | Inbound : partout où il est routé |

## Schéma A : DNS d'entreprise en premier

Les VNets sont en *Custom DNS*, pointés vers les contrôleurs de domaine.

```
Ressource ─► DC ─┬─ corp.exemple.fr  → répond (zone AD)
                 ├─ privatelink.*    → redirige vers l'inbound endpoint → zones liées au hub
                 └─ le reste         → Internet ou 168.63.129.16
```

Les zones privées doivent alors être liées **au VNet du resolver**, pas aux spokes.

## Schéma B : DNS Azure en premier

Les spokes restent en DNS par défaut, avec un *forwarding ruleset* lié.

```
Ressource ─► 168.63.129.16 ─┬─ zones privées liées au spoke → répond
                            ├─ corp.exemple.fr → ruleset → DNS d'entreprise
                            └─ le reste → Internet
```

Les zones privées doivent alors être liées **à chaque spoke**.

## Comment savoir dans quel schéma on est

```bash
az network vnet show -g <rg> -n <vnet> --query dhcpOptions.dnsServers   # vide = DNS Azure
cat /etc/resolv.conf                                                    # depuis la ressource elle-même
```

## Pièges

- **Zone liée au mauvais VNet** : en schéma A, lier la zone au spoke ne sert à rien. C'est `168.63.129.16`, interrogé depuis le VNet du resolver, qui doit la voir.
- **Domaine public utilisé en interne** ([[dns-split-horizon|split-horizon]]) : tout résolveur sur le chemin doit envoyer ce domaine vers le DNS interne, sinon il obtient `NXDOMAIN` depuis Internet.
- **Redirection oubliée côté on-prem** : les noms Azure privés se résolvent depuis Azure mais pas depuis les postes.
- Le **peering ne partage pas le DNS** : chaque VNet a son propre réglage.

## Voir aussi

- [[vnet-dns-azure-168-63-129-16]] · [[private-dns-zone]] · [[dns-private-resolver-azure]]
- [[service-endpoint-vs-private-endpoint]]
