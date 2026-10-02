---
tags: [observability, azure, reseau, vnet, dns, hybride]
---

# Chemin réseau vers un collecteur OTel privé (Web App, autre souscription, on-prem)

Liste de tout ce qui doit être en place pour qu'un émetteur hors de l'env Container Apps atteigne un collecteur dans un **env interne**. Contexte : [[collecteur-otel-central-acces-reseau]].

## Schéma

```
On-prem ──ExpressRoute/VPN──► HUB (gateway, firewall, DNS resolver) ──peering──► spoke "obs"
WebApp (sub B) ──VNet Integration──► HUB ────────────────────────────────────────┤ env interne
Container apps (env existant) ── sortie VNet ────────────────────────────────────┘ └ otel-collector
```

## Les 5 briques, dans l'ordre du trajet

1. **Faire sortir l'émetteur dans le VNet**
   - Web App : [[vnet-integration-subnet-delegation|VNet Integration]] (subnet délégué `Microsoft.Web/serverFarms`).
   - On-prem : [[vpn-gateway-expressroute|VPN / ExpressRoute]] arrivant dans le hub.
2. **Relier les réseaux** : [[vnet-peering|peering]] vers le hub ([[hub-and-spoke]]). Le peering n'étant pas transitif, le trafic spoke ↔ spoke passe par le firewall (UDR). Pour l'on-prem : *gateway transit* (« allow gateway transit » côté hub, « use remote gateways » côté spoke) afin que la plage du spoke soit annoncée.
3. **Autoriser le flux**
   - **Firewall** du hub (et firewalls on-prem) : source → subnet de l'env `obs`, en **TCP 443**. Le HTTPS passe mieux les équipements on-prem que le gRPC 4317.
   - **NSG** du subnet `obs` : autoriser l'entrée.
4. **Résoudre le nom** (l'étape la plus oubliée)
   - Le nom court `otel-collector` **ne marche qu'à l'intérieur de l'env** → utiliser un FQDN.
   - [[private-dns-zone|Zone DNS privée]] au nom du `defaultDomain` de l'env, enregistrement A **`*` → `staticIp`** de l'env, liée aux VNets concernés (lien cross-souscription possible dans le même tenant).
   - On-prem : **redirection conditionnelle** du DNS on-prem vers l'*inbound endpoint* de l'**Azure DNS Private Resolver** du hub.
   - Mieux : un **nom stable** `otel.<domaine-interne>` dans le DNS d'entreprise → `staticIp`, lié à la container app avec un **certificat de la PKI interne** (sinon 404, voir [[container-apps-ingress-dns-interne]]). Les clients doivent faire confiance à l'autorité de certification.
5. **Exposer le collecteur** : ingress `external: true` (dans un env interne = visible du VNet seulement), `transport: 'http'`, `targetPort: 4318` → `https://otel-collector.<domaine>`. Receiver avec authentification ([[opentelemetry-collector-config]]).

## Tester depuis la Web App (console Kudu)

```bash
nameresolver otel-collector.<defaultDomain>           # doit renvoyer une IP privée
curl -X POST https://otel-collector.<defaultDomain>/v1/traces   # 401 sans jeton = le réseau passe
```

Lecture des symptômes : nom non résolu → DNS ; timeout → route / firewall / NSG ; 404 → domaine non lié à l'app ; 401 → tout va bien côté réseau.

## Coût réseau

Les données **entrant** dans Azure (y compris via ExpressRoute) ne sont pas facturées. Le coût vient de la capacité de la gateway et surtout de **l'ingestion côté backend** → filtrer tôt ([[opentelemetry-collecteurs-multi-niveaux]]).

## Voir aussi

- [[collecteur-otel-central-acces-reseau]]
- [[private-dns-zone]] · [[hub-and-spoke]] · [[routage-asymetrique]]
