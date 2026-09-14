---
tags: [observability, opentelemetry, otlp, instrumentation]
---

# OpenTelemetry

**OpenTelemetry** (OTel) est un **standard ouvert** d'instrumentation : un format et des SDK pour produire [[observabilite-piliers|métriques, logs et traces]] de façon **agnostique du backend**.

Intérêt principal : **instrumenter une fois, choisir le backend après**. On redirige les données vers [[appinsights-principe|Application Insights]], Prometheus, [[grafana-tempo|Tempo]]… sans retoucher le code → [[vendor-lock-in|lock-in]] faible sur la partie coûteuse.

## Les 3 couches

```
[ App instrumentée ] → [ Collector ] → [ Backend(s) ]
   (produit OTLP)        (route/filtre)   (App Insights, Prometheus…)
```

- **App** : produit de l'**OTLP** (gRPC 4317 / HTTP 4318). Elle ignore la destination.
- [[opentelemetry-collector|Collector]] : reçoit, filtre/enrichit/échantillonne, exporte.
- **Backend** : stocke et affiche.

Changer de backend = reconfigurer le Collector, pas le code.

## Niveaux d'instrumentation (impact code)

- **Niveau 0 — zéro code** : agent runtime (ex. `-javaagent:opentelemetry-javaagent.jar`, `opentelemetry-instrument python app.py`).
- **Niveau 1 — bootstrap** : ~1 à 5 lignes au démarrage, auto-instrumente les libs courantes.
- **Niveau 2 — manuel** : spans/métriques métier, uniquement là où c'est utile.

## Voir aussi

- [[opentelemetry-collector]]
- [[container-apps-otel-agent]]
- [[correlation-traces-distribuees]]
- [[observabilite-piliers]]
