---
tags: [azure, reseau, vnet, app-service, paas]
---

# VNet Integration et délégation de subnet

La **VNet Integration** permet à un service PaaS (App Service, Functions…) d'**envoyer ses requêtes sortantes depuis un VNet**, pour joindre des ressources privées (base de données en private endpoint, API interne, on-prem).

## Le point qui embrouille : le sens du trafic

Un service PaaS a **deux portes**, qui se configurent **séparément** :

```
         ENTRÉE (qui peut m'appeler ?)          SORTIE (qu'est-ce que je peux appeler ?)
Client ───────────────> [ App Service ] ───────────────> SQL privé, API interne
         Private Endpoint                       VNet Integration
         Access restrictions
```

| Je veux… | Mécanisme |
|----------|-----------|
| que mon App Service **appelle** une ressource privée | **VNet Integration** (sortie) |
| que mon App Service **ne soit joignable qu'en privé** | **Private Endpoint** sur l'App Service (entrée), voir [[service-endpoint-vs-private-endpoint]] |
| les deux | Les deux, sur **deux subnets différents** |

⚠️ Activer la VNet Integration **ne rend pas** l'application privée : elle reste joignable depuis Internet.

## La délégation de subnet

La VNet Integration exige un **subnet délégué** : on le réserve à un service précis (`Microsoft.Web/serverFarms` pour App Service), qui y crée lui-même ses cartes réseau.

- Un subnet délégué **ne peut rien accueillir d'autre**.
- Le dimensionner selon le nombre d'instances maximum (scale-out). `/26` est une taille confortable pour App Service.
- D'autres services délèguent aussi : Container Apps, SQL Managed Instance, NetApp…

## Ce que la sortie peut atteindre

Une fois intégrée, l'application utilise le routage et le DNS du VNet :

- les [[vnet-routage-udr|UDR]] s'appliquent, ce qui permet de forcer la sortie via un firewall ;
- les [[private-dns-zone|zones DNS privées]] sont résolues ;
- les VNets appairés et l'on-prem sont joignables.

## Voir aussi

- [[vnet-subnets-adressage#Subnets dédiés et délégués]]
- [[azure-app-services]]
- [[sortie-internet-nat-gateway]] : fixer l'IP de sortie d'une App Service
