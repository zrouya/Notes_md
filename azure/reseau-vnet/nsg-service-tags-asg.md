---
tags: [azure, reseau, vnet, nsg]
---

# NSG — Service Tags et ASG

Deux mécanismes pour écrire des règles [[network-security-group-nsg-azure|NSG]] **par nom** plutôt que par adresse IP. Les IP changent ; les noms restent.

## Service Tags : « les IP de tel service Microsoft »

Un Service Tag est un **nom qui représente une liste d'IP maintenue par Microsoft**.

```
Source: ApiManagement      Dest: VirtualNetwork   Port: 3443   Allow   # plan de gestion APIM
Source: VirtualNetwork     Dest: Storage.WestEurope  Port: 443 Allow   # sortie vers le Storage de la région
```

| Tag | Représente |
|-----|------------|
| `Internet` | Toute IP publique hors Azure |
| `VirtualNetwork` | Le VNet, **les VNets appairés** et l'on-prem connecté |
| `AzureLoadBalancer` | Les sondes de santé Azure (`168.63.129.16`) |
| `ApiManagement`, `AppService`, `Sql`, `Storage`… | Les IP publiques de ce service |
| `Storage.WestEurope` | Même chose, limité à une région |

Intérêt : pas de liste d'IP à maintenir à la main, Microsoft la tient à jour.

## ASG (Application Security Group) : « les machines qui ont tel rôle »

Un ASG est une **étiquette qu'on pose sur des cartes réseau**. On écrit ensuite les règles entre étiquettes :

```
Source: asg-web    Dest: asg-api    Port: 443    Allow
Source: asg-api    Dest: asg-db     Port: 1433   Allow
```

- On ajoute une VM à `asg-web` et elle hérite de toutes les règles, sans toucher au NSG.
- Contrainte : les NIC d'un même ASG doivent être **dans le même VNet**.

## Quand utiliser quoi

| Besoin | Outil |
|--------|-------|
| Autoriser / bloquer un **service Azure** | Service Tag |
| Cloisonner **mes propres machines** par rôle | ASG |
| Une IP ou une plage précise (partenaire, bureau) | CIDR classique |

## Voir aussi

- [[network-security-group-nsg-azure]]
- [[cidr-notation]]
