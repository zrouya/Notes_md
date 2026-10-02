---
tags: [azure, container-apps, ingress, dns, bicep]
---

# Container Apps : ingress et DNS interne

Comment une container app est joignable : le flag `external` de son ingress, le transport (HTTP ou TCP), et les noms DNS qu'Azure lui donne automatiquement.

## Le flag `external` de l'ingress

Il ne parle **pas** d'Internet. Il signifie « **publier l'app sur la porte de l'Environment** », quelle que soit cette porte (publique ou privée, voir [[container-apps-environnement-externe-vs-interne]]).

- `external: true` → joignable par la porte de l'env.
- `external: false` → joignable **uniquement par les autres apps du même env**.

## Transport HTTP vs TCP

```bicep
ingress: {
  external: false
  transport: 'tcp'      // ou 'http' (défaut, 'auto')
  targetPort: 4317      // port sur lequel écoute le conteneur
  exposedPort: 4317     // port exposé aux clients (TCP uniquement)
  additionalPortMappings: [
    { external: false, targetPort: 4318, exposedPort: 4318 }
  ]
}
```

| | `http` | `tcp` |
|---|---|---|
| Port côté client | 80 / 443 | `exposedPort` au choix |
| TLS | **Terminé par Envoy** (HTTPS auto) | ❌ non géré → trafic en clair (ou TLS dans l'app) |
| Routage par nom (Host) | ✅ | ❌ |
| Prérequis | — | Env intégré à un VNet |

## Les noms DNS automatiques

Dans un même env, chaque app avec ingress est joignable par :

- son **nom court** = le `name` de la container app → `otel-collector` (⚠️ **seulement depuis l'intérieur de l'env**) ;
- son **FQDN** :
  - ingress interne : `<app>.internal.<defaultDomain>`
  - ingress externe : `<app>.<defaultDomain>`

Exemple : `http://otel-collector:4317` = nom de l'app + `exposedPort` + `http` car TCP sans TLS. **Rien ne le déclare explicitement** : c'est déduit.

## Le rendre explicite (output Bicep)

```bicep
output otlpGrpcEndpoint string = 'http://${otelCollector.properties.configuration.ingress.fqdn}:4317'
```

→ passer cet output en paramètre aux modules consommateurs plutôt que de coder l'URL en dur.

## Nom de domaine personnalisé

Pour un nom stable (ex. `otel.mondomaine.interne`) :
- enregistrement DNS → `staticIp` de l'env ;
- **lier le domaine à la container app** + certificat. Envoy route par l'en-tête **Host** : sans liaison → **404**.
- Les certificats managés exigent une validation DNS publique → pour un nom interne, **certificat de la PKI interne**.

## Voir aussi

- [[container-apps-environnement-externe-vs-interne]]
- [[container-apps-otel-collector-self-hosted]]
- [[private-dns-zone]]
