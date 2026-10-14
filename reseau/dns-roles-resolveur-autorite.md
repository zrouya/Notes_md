---
tags: [reseau, dns, resolution, generique]
---

# DNS : client, résolveur récursif, serveur faisant autorité

Une résolution DNS fait intervenir trois rôles distincts. Savoir **qui pose la question à qui** est la clé de presque tous les problèmes de DNS.

> [!abstract] Portée : notion générique (réseau)
> Déclinaison Azure : le résolveur récursif par défaut d'un VNet est `168.63.129.16`, voir [[vnet-dns-azure-168-63-129-16]].

## Les trois rôles

| Rôle | Ce qu'il fait | Exemples |
|------|---------------|----------|
| **Client** (*stub resolver*) | Pose une seule question et attend une réponse toute faite | La bibliothèque réseau de l'OS ou du conteneur |
| **Résolveur récursif** | Fait toute la recherche pour le client, en partant de la racine, et met les réponses en cache | DNS de la box, contrôleur de domaine AD, `8.8.8.8`, `1.1.1.1` |
| **Serveur faisant autorité** (*authoritative*) | Détient la réponse officielle pour ses zones, et seulement pour elles | Les serveurs `NS` d'un domaine |

Un même serveur peut cumuler les rôles : un contrôleur de domaine fait autorité pour la zone AD et sert de résolveur récursif pour le reste.

## Quel résolveur un client utilise-t-il ?

Celui qu'on lui a configuré, et lui seul :

```bash
cat /etc/resolv.conf                 # Linux, conteneur : nameserver, search
ipconfig /all                        # Windows : « Serveurs DNS »
Get-DnsClientServerAddress           # PowerShell
```

En général, ce réglage arrive par [[serveur-dhcp|DHCP]]. Changer le DNS d'un réseau n'a donc d'effet qu'au **renouvellement du bail** ou au redémarrage des machines.

Le champ `search` (suffixes DNS) complète les noms courts : avec `search exemple.local`, `nslookup app` interroge aussi `app.exemple.local`.

## Le cache, à chaque étage

Le client, le résolveur et parfois l'application gardent les réponses pendant le [[dns-ttl-migration|TTL]]. Une réponse négative (« nom inexistant », `NXDOMAIN`) est aussi mise en cache : un nom créé juste après un échec peut rester introuvable quelques minutes.

## Diagnostic

```bash
nslookup app.exemple.fr              # avec le résolveur configuré
nslookup app.exemple.fr 10.0.0.4     # en forçant un résolveur précis
dig +trace app.exemple.fr            # en partant de la racine, sans cache
```

Comparer les réponses de deux résolveurs révèle vite un problème de [[dns-redirection-conditionnelle|redirection]] ou de [[dns-split-horizon|split-horizon]].

## Voir aussi

- [[Domain Name System (DNS)]] · [[DNS record]]
- [[dns-redirection-conditionnelle]] · [[dns-split-horizon]]
- [[vnet-dns-azure-168-63-129-16]] : le cas Azure
