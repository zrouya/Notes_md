---
tags: [azure, container-apps, reseau, vnet, ingress, securite]
---

# Container Apps Environment : mode externe vs interne

Le mode d'un Container Apps Environment décide **d'une seule chose : où se trouve sa porte d'entrée réseau**. Contre-intuitif : « externe » ne veut pas dire « plus facile d'accès », mais « la porte donne sur Internet ».

## L'idée clé : une seule porte d'entrée

Chaque Environment a **un unique point d'entrée** : un load balancer avec une IP statique, devant le proxy Envoy qui répartit vers les apps.

| Mode | La porte est… | Le `defaultDomain` se résout… |
|---|---|---|
| **Externe** (défaut si `internal` absent) | une **IP publique** (côté Internet) | par le DNS public |
| **Interne** (`internal: true`) | une **IP privée dans le subnet** (côté VNet) | nulle part en public → [[private-dns-zone\|zone DNS privée]] obligatoire |

⚠️ En mode externe, **il n'y a aucune porte côté VNet** : rien n'écoute en entrée sur une IP privée.

```bicep
vnetConfiguration: {
  infrastructureSubnetId: subnetId
  internal: true        // absent = externe !
}
```

## Analogie du bâtiment

- **Environment** = un bâtiment avec **une seule porte principale**.
- **Externe** : la porte donne **sur la rue** (Internet).
- **Interne** : la porte donne **sur le couloir privé de l'entreprise** (VNet + réseaux peerés + on-prem).
- **Ingress d'app `external: true`** : le bureau est accessible depuis la porte principale.
- **Ingress d'app `external: false`** : seuls les occupants du bâtiment (les autres apps) y accèdent.

Détail du flag d'ingress : [[container-apps-ingress-dns-interne]].

## Qui peut joindre quoi

| | Internet | VNet / peering / on-prem | Autre app du même env |
|---|---|---|---|
| Env externe + ingress externe | ✅ | ⚠️ seulement via l'IP **publique** | ✅ |
| Env externe + ingress interne | ❌ | ❌ | ✅ |
| Env interne + ingress externe | ❌ | ✅ (IP privée) | ✅ |
| Env interne + ingress interne | ❌ | ❌ | ✅ |

## La sortie est identique dans les deux modes

Le mode ne concerne **que l'entrant**. Dans les deux cas, les apps **sortent** par le subnet : elles joignent le VNet, les peerings, les private endpoints, l'on-prem (sous réserve de NSG / firewall / [[vnet-routage-udr|UDR]]).
→ Une app d'un env externe **peut appeler** un service dans un env interne. C'est l'inverse qui est impossible en privé.

## Pourquoi « externe » semble plus simple

- Vrai **pour publier sur Internet** : DNS public et certificat automatiques (cas des tutos).
- Faux **en landing zone / hybride** : l'interne coûte juste une zone DNS privée, mais donne une entrée privée et **zéro exposition publique**. On publie ensuite sur Internet via Application Gateway (WAF) ou Front Door Premium + Private Link.
- → **Interne = choix standard en landing zone.**

## À retenir

- **Immuable** : le mode se fixe à la création (voir [[container-apps-choix-irreversibles]]). Changer = recréer l'env et redéployer toutes les apps.
- **Sécurité** : en env externe, toute app en ingress `external: true` est **directement sur Internet** (seules les `ipSecurityRestrictions` la protègent) → à auditer.
- Les env récents en *workload profiles* peuvent avoir un **private endpoint** + `publicNetworkAccess: Disabled`, ce qui brouille la distinction (pas possible en *Consumption* legacy).

## Voir aussi

- [[container-apps-ingress-dns-interne]]
- [[collecteur-otel-central-acces-reseau]] — cas concret où le mode externe bloque
- [[vnet-integration-subnet-delegation]] — même logique entrée ≠ sortie côté App Service
- [[private-dns-zone]]
