---
tags: [observability, opentelemetry, sdk, otlp, instrumentation]
---

# Configurer un SDK OpenTelemetry pour exporter en OTLP

Côté application, le SDK [[opentelemetry|OTel]] produit la télémétrie et l'envoie en OTLP. Il se configure **par variables d'environnement standard**, identiques dans tous les langages.

## Variables `OTEL_*`

| Variable | Exemple | Rôle |
|---|---|---|
| `OTEL_SERVICE_NAME` | `mon-api` | Nom du service dans les traces (**indispensable**) |
| `OTEL_RESOURCE_ATTRIBUTES` | `deployment.environment=prd,service.namespace=entoria` | Attributs d'identité, pour filtrer/router |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | `https://otel-collector.xxx` | URL du collecteur |
| `OTEL_EXPORTER_OTLP_PROTOCOL` | `grpc` ou `http/protobuf` | Protocole |
| `OTEL_EXPORTER_OTLP_HEADERS` | `Authorization=Bearer xxx` | Auth (via secret / Key Vault) |

Sur Container Apps avec l'[[container-apps-otel-agent|agent managé]], endpoint et protocole sont **injectés automatiquement**.

## Mise en place par langage

| Langage | Mise en place |
|---|---|
| **.NET (Core/5+)** | `OpenTelemetry.Extensions.Hosting` + `OpenTelemetry.Exporter.OpenTelemetryProtocol` → `AddOpenTelemetry().WithTracing(...).WithMetrics(...).WithLogging(...).UseOtlpExporter()` |
| **.NET Framework 4.x** | Voir [[opentelemetry-dotnet-framework]] |
| **Java** | `JAVA_TOOL_OPTIONS=-javaagent:/otel/opentelemetry-javaagent.jar` (zéro code) |
| **Node.js** | `NODE_OPTIONS=--require @opentelemetry/auto-instrumentations-node/register` |
| **Python** | `opentelemetry-distro` + `opentelemetry-exporter-otlp`, lancer via `opentelemetry-instrument python app.py` |

## Pièges

- **Ne pas cumuler** l'exporteur OTLP et l'exporteur Azure Monitor (`UseAzureMonitor()`) → données en double.
- Endpoint **dans le code** en `http/protobuf` : il faut l'URL complète par signal (`.../v1/traces`). Via la variable d'env, le SDK ajoute le chemin lui-même.

## Voir aussi

- [[opentelemetry]]
- [[opentelemetry-dotnet-framework]]
- [[container-apps-otel-agent]]
