---
tags: [observability, opentelemetry, collector, otlp]
---

# OpenTelemetry Collector

Couche intermédiaire d'[[opentelemetry|OpenTelemetry]] : il **reçoit l'OTLP** des apps, peut le **traiter** (filtrer, enrichir, échantillonner, rédiger des attributs), puis l'**exporte** vers un ou plusieurs backends.

Porte la portabilité : les apps ignorent la destination, seul le Collector la connaît.

## Pipeline

```
receivers → processors → exporters
```

- **receivers** : entrée (OTLP gRPC/HTTP…)
- **processors** : batch, filtrage, **tail sampling**, transformation
- **exporters** : sortie vers [[appinsights-principe|App Insights]], Prometheus, [[grafana-tempo|Tempo]]…

## Collector managé vs complet

- Un **agent managé** (ex. [[container-apps-otel-agent]]) est un Collector **bridé** : destinations + routage par signal, mais pas de pipeline arbitraire.
- Pour du traitement avancé : **agent managé → Collector auto-hébergé (OTLP)** portant la vraie pipeline, qui fan-out vers les backends.

## Voir aussi

- [[opentelemetry]]
- [[container-apps-otel-agent]]
- [[stack-lgtm]]
