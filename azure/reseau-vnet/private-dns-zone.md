---
tags: [azure, reseau, vnet, dns, private-link]
---

# Private DNS Zone et DNS dans un VNet

Un [[service-endpoint-vs-private-endpoint|Private Endpoint]] donne une IP privée au service, mais les applications l'appellent **par son nom**. Il faut donc que ce nom se résolve vers l'IP privée **depuis l'intérieur du VNet** : c'est le rôle des Private DNS Zones.

## Le mécanisme, étape par étape

Quand on crée un Private Endpoint pour `monstorage`, Azure modifie le DNS **public** :

```
monstorage.blob.core.windows.net
   CNAME -> monstorage.privatelink.blob.core.windows.net
```

Ensuite, **tout dépend de l'endroit d'où l'on pose la question** :

| Depuis | Résolution de `monstorage.privatelink.blob...` | Résultat |
|--------|-----------------------------------------------|----------|
| Le VNet, avec la zone privée liée | Enregistrement A dans la **Private DNS Zone** | `10.10.4.5` ✅ |
| Internet | DNS public | IP publique (refusée si l'accès public est coupé) |

La **Private DNS Zone** `privatelink.blob.core.windows.net` contient l'enregistrement `monstorage → 10.10.4.5`. Elle doit être **liée** (*virtual network link*) à chaque VNet qui doit la voir.

Rappel des types d'enregistrement (A, CNAME) : [[DNS record]].

## Le DNS par défaut d'un VNet

- Chaque VNet utilise par défaut le **DNS Azure**, à l'IP virtuelle `168.63.129.16`, joignable uniquement depuis Azure.
- C'est lui qui consulte les Private DNS Zones liées au VNet.
- On peut remplacer ce DNS par ses propres serveurs (paramètre *DNS servers* du VNet). Ces serveurs doivent alors **relayer** vers `168.63.129.16` pour les zones `privatelink`.

## Et depuis l'on-prem ?

Le DNS du bureau ne connaît pas les zones privées Azure et ne peut pas joindre `168.63.129.16`. La solution est l'**Azure DNS Private Resolver**, placé dans le hub :

```
Poste on-prem → DNS on-prem → (redirection conditionnelle *.privatelink.*) → Private Resolver (IP 10.0.x.x) → zone privée → 10.10.4.5
```

## Cas d'un Container Apps Environment interne

Pas de private endpoint ici, mais le même besoin : le `defaultDomain` d'un env interne n'est résolu nulle part en public. Créer une zone privée **au nom du `defaultDomain`** avec un enregistrement A **`*` → `staticIp` de l'env** (le wildcard couvre toutes les apps). Voir [[container-apps-environnement-externe-vs-interne]].

## Symptôme typique d'un DNS mal configuré

« Ça marche avec l'IP, pas avec le nom », ou le nom résout vers une **IP publique** et la connexion est refusée ou part en timeout.

```bash
nslookup monstorage.blob.core.windows.net   # doit renvoyer 10.x.x.x depuis le VNet
```

## Voir aussi

- [[Domain Name System (DNS)]] · [[DNS record]]
- [[hub-and-spoke]] : zones et resolver centralisés dans le hub
- [[diagnostic-par-couche]]
