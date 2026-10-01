---
tags: [azure, reseau, vnet, subnet, ip]
---

# VNet — espace d'adresses et subnets

On donne au VNet une **plage d'adresses IP privées** (son *address space*), puis on la découpe en **subnets**. Chaque ressource reçoit une IP d'un subnet.

## Exemple

```
VNet  vnet-app-prod   10.10.0.0/16        -> 65 536 adresses au total
 ├── snet-app         10.10.1.0/24        -> 251 IP utilisables
 ├── snet-data        10.10.2.0/24
 ├── snet-apim        10.10.3.0/27        -> subnet dédié à APIM
 ├── snet-pe          10.10.4.0/26        -> private endpoints
 └── GatewaySubnet    10.10.255.0/27      -> nom imposé par Azure
```

Lecture : `/16` signifie « les 16 premiers bits sont fixes », donc tout ce qui commence par `10.10.` appartient au VNet. Voir [[cidr-notation]].

## Les règles

- **Plages privées** ([[cidr-notation#Plages privées (RFC 1918)|RFC 1918]]) : `10.x`, `172.16-31.x` ou `192.168.x`.
- Les subnets doivent être **inclus** dans l'address space et **ne pas se chevaucher** entre eux.
- **5 IP réservées par subnet** : les 4 premières et la dernière. Un `/29` (8 adresses) n'en laisse que 3. Voir [[Sous-réseau#Spécificité Azure]].
- On peut ajouter un 2e address space à un VNet plus tard. Agrandir un subnet déjà occupé est en revanche compliqué : **prévoir large**.

## Le piège n°1 : le chevauchement

Si deux réseaux qui devront un jour communiquer utilisent la même plage (par exemple deux VNets en `10.0.0.0/16`, ou un VNet et le réseau du bureau), **on ne pourra jamais les relier** : un routeur ne sait pas dans lequel des deux envoyer un paquet destiné à `10.0.1.5`.

→ Tenir un **plan d'adressage global** (qui a quelle plage) avant de créer quoi que ce soit.

## Subnets dédiés et délégués

Certains services Azure veulent **leur propre subnet** :

| Subnet | Pour | Particularité |
|--------|------|---------------|
| `GatewaySubnet` | VPN / ExpressRoute Gateway | Nom imposé |
| `AzureFirewallSubnet` | Azure Firewall | Nom imposé, `/26` minimum |
| `AzureBastionSubnet` | Azure Bastion | Nom imposé |
| subnet APIM | API Management en VNet | Dédié, NSG spécifique |
| subnet délégué | App Service, Container Apps… | Voir [[vnet-integration-subnet-delegation]] |

**Déléguer** un subnet, c'est dire à Azure : « ce subnet appartient au service X, lui seul y crée des cartes réseau ».

## Voir aussi

- [[vnet-principe]]
- [[Sous-réseau]] · [[Masque de sous-réseau]]
- [[vnet-peering]] : là où le chevauchement fait mal
