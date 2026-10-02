---
tags: [observability, opentelemetry, collector, yaml, snippet]
---

# Config type d'un OpenTelemetry Collector

Fichier `config.yaml` commenté d'un [[opentelemetry-collector|Collector]] : réception OTLP, protections, filtrage FinOps, export. Hébergement Azure : [[container-apps-otel-collector-self-hosted]].

## Exemple

```yaml
extensions:
  health_check: { endpoint: 0.0.0.0:13133 }        # sonde de vie
  bearertokenauth: { token: "${env:OTLP_INGEST_TOKEN}" }  # auth des émetteurs

receivers:
  otlp:
    protocols:
      grpc: { endpoint: 0.0.0.0:4317 }
      http:
        endpoint: 0.0.0.0:4318
        auth: { authenticator: bearertokenauth }

processors:
  memory_limiter:            # TOUJOURS en premier : protège contre l'OOM
    check_interval: 1s
    limit_percentage: 80
    spike_limit_percentage: 20
  filter/noise:              # on jette AVANT d'envoyer (= avant de payer)
    error_mode: ignore
    traces:
      span: [ 'attributes["url.path"] == "/health"' ]
    logs:
      log_record: [ 'severity_number < SEVERITY_NUMBER_INFO' ]
  resource:
    attributes:
      - { key: deployment.environment, value: "${env:ENVIRONMENT}", action: upsert }
  batch: {}                  # regroupe les envois (en dernier)

exporters:
  azuremonitor: { connection_string: "${env:APPLICATIONINSIGHTS_CONNECTION_STRING}" }
  # otlphttp: { endpoint: https://backend, headers: { x-api-key: "${env:KEY}" } }

service:
  extensions: [health_check, bearertokenauth]
  pipelines:
    traces:  { receivers: [otlp], processors: [memory_limiter, filter/noise, resource, batch], exporters: [azuremonitor] }
    logs:    { receivers: [otlp], processors: [memory_limiter, filter/noise, resource, batch], exporters: [azuremonitor] }
    metrics: { receivers: [otlp], processors: [memory_limiter, resource, batch],               exporters: [azuremonitor] }
```

## Détails

- **Ports standard OTLP** : 4317 = gRPC, 4318 = HTTP (`/v1/traces`, `/v1/logs`, `/v1/metrics`).
- **Ordre des processors** : `memory_limiter` d'abord, `batch` à la fin.
- **`${env:VAR}`** : les secrets viennent de variables d'environnement, jamais en dur.
- **Une extension doit être déclarée ET activée** dans `service.extensions`.
- **Auth** : un jeton unique suffit pour démarrer ; avec beaucoup de sources → un jeton par source ou **mTLS**.
- **Tail sampling** (`tail_sampling`) : faux si plusieurs replicas reçoivent les spans d'une même trace → voir [[opentelemetry-collecteurs-multi-niveaux]].

## Voir aussi

- [[opentelemetry-collector]]
- [[opentelemetry-collecteurs-multi-niveaux]]
- [[log-analytics-cout]]
