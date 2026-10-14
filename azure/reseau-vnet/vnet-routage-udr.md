---
tags: [azure, reseau, vnet, routage]
---

# VNet — routage, routes système et User Defined Routes

Le **routage** répond à une seule question : « ce paquet va vers telle IP, **par où** je l'envoie ? ». Azure y répond avec une table de routage attachée à chaque subnet.

> [!abstract] Portée : spécifique Azure
> Concept général : [[routes-statiques-vs-dynamiques]]. Une UDR est une **route statique**, une route système correspond aux routes **connectées**, et une route BGP est une route **dynamique** (voir [[routes-bgp-azure]]).

## Les routes système (créées automatiquement)

Sans rien configurer, chaque subnet a déjà ces routes :

| Destination | Prochain saut (*next hop*) | Effet |
|-------------|----------------------------|-------|
| Plage du VNet (ex. `10.10.0.0/16`) | `VirtualNetwork` | Tous les subnets se voient |
| Plages des VNets appairés | `VNetPeering` | Apparaît quand on crée un [[vnet-peering]] |
| Plages on-prem | `VirtualNetworkGateway` | Apprises en BGP avec un VPN / ExpressRoute, voir [[routes-bgp-azure]] |
| `0.0.0.0/0` (tout le reste) | `Internet` | Sortie vers Internet |
| Autres plages privées non utilisées | `None` | Paquet jeté |

`0.0.0.0/0` est la **route par défaut** : elle correspond à toutes les adresses, mais c'est la moins précise, donc la dernière choisie.

## Comment Azure choisit une route

1. **Le préfixe le plus long gagne** (le plus précis). Pour `10.10.2.4`, la route `10.10.2.0/24` bat `10.10.0.0/16`, qui bat `0.0.0.0/0`.
2. À préfixe égal : **UDR > BGP > système**.

C'est la même logique que [[Routage réseau|n'importe quel routeur]].

## Les UDR (User Defined Routes)

On crée une **Route Table**, on y ajoute des routes, puis on **l'associe à un subnet**. Les routes s'appliquent au trafic qui **sort** de ce subnet.

```
Route Table  rt-spoke-app  (associée à snet-app)
  0.0.0.0/0    -> VirtualAppliance  10.0.1.4   # IP privée du firewall du hub
```

Cas d'usage typique, le **forced tunneling** : forcer tout le trafic à passer par un firewall pour l'inspecter, au lieu de sortir directement sur Internet.

Types de next hop possibles : `VirtualAppliance` (une IP : firewall, NVA), `VirtualNetworkGateway`, `Internet`, `VirtualNetwork`, `None` (jeter).

Dans le portail, la colonne **Route table** d'un subnet indique la route table associée, donc ses UDR. `None` = aucune UDR. Les routes système et BGP n'y figurent pas : elles ne sont visibles que dans *Effective routes*.

L'option **Propagate gateway routes** d'une route table laisse arriver (ou non) les [[routes-bgp-azure|routes BGP]] de la gateway dans les subnets associés.

## Pièges

- Une route table ne s'applique **qu'aux subnets auxquels elle est associée**, pas au VNet entier.
- Sur un subnet de service PaaS, préférer une **route précise** (la plage visée) à un `0.0.0.0/0` : la route par défaut détourne aussi le trafic de gestion de la plateforme.
- Certains services ne prennent pas en charge l'UDR de leur subnet (ex. Container Apps *Consumption only*) : la route table existe, mais rien ne garantit son effet.
- Router l'aller par un firewall sans router le retour, c'est le [[routage-asymetrique]].
- Certains subnets de service (APIM, Gateway) supportent mal un `0.0.0.0/0` vers un firewall : vérifier la doc du service.
- Diagnostic : portail → carte réseau de la VM → **Effective routes**, qui affiche la table réellement appliquée.

## Voir aussi

- [[Routage réseau]] · [[Passerelle réseau]] · [[routes-statiques-vs-dynamiques]]
- [[routes-bgp-azure]] : l'autre source de routes, apprises de l'on-prem
- [[hub-and-spoke]] : où les UDR servent le plus
- [[network-security-group-nsg-azure]] : le routage choisit le chemin, le NSG autorise ou bloque
