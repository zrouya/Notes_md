---
tags: [azure, container-apps, opentelemetry, collector, bicep]
---

# OTel Collector auto-hébergé en Container App

Déployer son propre [[opentelemetry-collector|OpenTelemetry Collector]] comme une container app, quand l'[[container-apps-otel-agent|agent managé]] ne suffit pas (filtrage, échantillonnage, routage fin). La config du collecteur elle-même : [[opentelemetry-collector-config]].

## Bicep (les éléments importants)

```bicep
resource otelCollector 'Microsoft.App/containerApps@2024-03-01' = {
  name: 'otel-collector'                  // = nom DNS court dans l'env
  properties: {
    managedEnvironmentId: env.id
    configuration: {
      secrets: [
        { name: 'otelcol-config', value: loadTextContent('otelcol/config.yaml') }
        { name: 'appi-connstr',   value: appInsightsConnectionString } // ou keyVaultUrl
      ]
      ingress: { external: false, transport: 'tcp', targetPort: 4317, exposedPort: 4317
                 additionalPortMappings: [ { external: false, targetPort: 4318, exposedPort: 4318 } ] }
    }
    template: {
      containers: [ {
        name: 'otelcol'
        image: '<acr>.azurecr.io/otel/opentelemetry-collector-contrib:<version-figée>'
        args: [ '--config=/etc/otelcol/config.yaml' ]
        env: [ { name: 'APPLICATIONINSIGHTS_CONNECTION_STRING', secretRef: 'appi-connstr' } ]
        resources: { cpu: json('0.5'), memory: '1Gi' }
        volumeMounts: [ { volumeName: 'config', mountPath: '/etc/otelcol' } ]
        probes: [ { type: 'Liveness', httpGet: { port: 13133, path: '/' } } ]
      } ]
      volumes: [ { name: 'config', storageType: 'Secret'
                   secrets: [ { secretRef: 'otelcol-config', path: 'config.yaml' } ] } ]
      scale: { minReplicas: 2, maxReplicas: 5 }
    }
  }
}
```

## Les astuces à retenir

- **Config montée depuis un secret** (`storageType: 'Secret'`) : le fichier YAML devient un fichier dans le conteneur, sans Azure Files.
- **`minReplicas` ≥ 1, jamais 0** : sinon perte de télémétrie et démarrages à froid.
- **Image contrib** (la *core* n'a ni `azuremonitor` ni `tail_sampling`), **copiée dans l'ACR** et **version figée** (breaking changes fréquents).
- **Health check** sur le port 13133 (extension `health_check`).
- **Surveiller le collecteur** : ses logs vont dans `ContainerAppConsoleLogs`, ses métriques internes (port 8888) exposent `otelcol_exporter_send_failed_*` (perte de données).

## Brancher les apps

D'où vient l'URL : [[container-apps-ingress-dns-interne]].
- **Directement** : `OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4317` sur chaque app.
- **Via l'agent managé** (hybride) : endpoint OTLP de l'agent = le collecteur.

## Piège : la dépendance circulaire

En hybride avec le collecteur **dans le même env** : l'env a besoin de l'URL du collecteur (`openTelemetryConfiguration`), et le collecteur a besoin de l'env (`managedEnvironmentId`) → Bicep refuse.
- Déployer en **deux passes** (env sans OTel → collecteur → redéploiement de l'env avec OTel), ou
- héberger le collecteur dans **un autre env** → plus de cycle (voir [[collecteur-otel-central-acces-reseau]]).

## Voir aussi

- [[opentelemetry-collector-config]]
- [[container-apps-otel-agent]]
- [[opentelemetry-collecteurs-multi-niveaux]]
