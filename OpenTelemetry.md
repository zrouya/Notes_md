
**OpenTelemetry** (OTel) est un **standard ouvert** d'instrumentation pour l'observabilité : il définit un format et des SDK pour produire des [[Observabilité - métriques, logs et traces|métriques, logs et traces]] de façon **agnostique du backend**.

Son intérêt principal : **instrumenter une fois, choisir le backend après**. On peut rediriger les données vers [[Application Insights]], [[Prometheus]], Tempo, etc. sans retoucher le code applicatif — ce qui réduit fortement le [[Vendor lock-in|lock-in]] au niveau du code.

## Les 3 couches

```
[ App instrumentée ] → [ Collector ] → [ Backend(s) ]
   (produit OTLP)        (route/filtre)   (App Insights, Prometheus…)
```

- **App** : produit des données au format **OTLP** (OpenTelemetry Protocol, gRPC port 4317 / HTTP port 4318). Elle ne sait pas où ça finit.
- [[OpenTelemetry Collector|Collector]] : reçoit l'OTLP, filtre/enrichit/échantillonne, puis exporte.
- **Backend** : stocke et affiche.

La portabilité vient de cette séparation : changer de backend = reconfigurer le Collector, pas le code.

## Niveaux d'instrumentation (impact code)

- **Niveau 0 — zéro code** : un agent runtime instrumente les libs connues (serveur HTTP, clients HTTP, drivers SQL). Ex : `-javaagent:opentelemetry-javaagent.jar`, `opentelemetry-instrument python app.py`.
- **Niveau 1 — bootstrap** : ~1 à 5 lignes au démarrage (ex. `configure_azure_monitor()` en Python, `.UseAzureMonitor()` en .NET). Auto-instrumente les libs courantes.
- **Niveau 2 — manuel** : spans custom et métriques métier, uniquement là où c'est utile. Incrémental.

## Voir aussi

- [[OpenTelemetry Collector]]
- [[Agent OpenTelemetry Azure Container Apps]]
- [[Observabilité - métriques, logs et traces]]
- [[Application Insights]]
