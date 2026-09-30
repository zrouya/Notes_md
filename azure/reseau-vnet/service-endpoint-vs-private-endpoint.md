---
tags: [azure, reseau, vnet, private-link, paas]
---

# Service Endpoint vs Private Endpoint

Les services PaaS (Storage, SQL, Key Vault…) **ne vivent pas dans votre VNet** : ils ont une **adresse publique** sur Internet. Ces deux mécanismes permettent de les joindre de façon privée, mais de manières très différentes.

## Le point de départ

```
VM 10.10.1.5  ──> Internet ──>  monstorage.blob.core.windows.net  (IP publique 20.x.x.x)
                                 ouvert au monde entier
```

## Service Endpoint : « un raccourci + un badge »

On active l'option sur **le subnet**, pour un type de service (`Microsoft.Storage`, par exemple).

- Le trafic du subnet vers ce service passe par le réseau Microsoft, sans sortir sur Internet.
- Le service voit arriver la requête **avec l'identité du subnet**. On peut alors régler son firewall sur « n'accepter que le subnet `snet-app` ».
- ⚠️ Le service garde **son IP publique**. Rien n'est ajouté dans le VNet.
- Ne fonctionne pas depuis l'on-prem ni depuis un VNet appairé non autorisé.

## Private Endpoint : « le service emménage chez vous »

On crée une **carte réseau dans un de vos subnets**, avec une **IP privée à vous**, qui représente **une instance précise** du service (ce storage-là, pas tous les storages).

```
VNet 10.10.0.0/16
 └── snet-pe
      └── pe-monstorage   10.10.4.5  ──(Private Link)──>  monstorage
```

- On peut **désactiver complètement l'accès public** du service.
- Joignable depuis les VNets appairés et l'on-prem, puisque c'est une IP privée comme une autre.
- ⚠️ Exige un **DNS privé** pour que le nom `monstorage.blob.core.windows.net` pointe vers `10.10.4.5` et non vers l'IP publique. Voir [[private-dns-zone]].

## Comparaison

| | Service Endpoint | Private Endpoint |
|--|------------------|------------------|
| IP du service | Publique | **Privée, dans votre VNet** |
| Accès public coupable | Non (filtré) | **Oui** |
| Granularité | Tout un type de service | Une instance précise |
| Depuis l'on-prem / un peering | Non | Oui |
| DNS à gérer | Non | **Oui** |
| Coût | Gratuit | Payant (à l'heure et au Go) |

**Recommandation actuelle : Private Endpoint.** Le Service Endpoint reste utile pour un besoin simple et gratuit.

## Sens du trafic

Le Private Endpoint sert à **entrer vers** un service. Pour qu'une App Service **sorte dans** le VNet, c'est un autre mécanisme : [[vnet-integration-subnet-delegation]].

## Voir aussi

- [[private-dns-zone]]
- [[network-security-group-nsg-azure]]
- [[azure-storage-accounts]] · [[azure-sql-database]]
