
Le **Collector** est la couche intermédiaire d'[[OpenTelemetry]] : il **reçoit l'OTLP** des apps, peut le **traiter** (filtrer, enrichir, échantillonner, rédiger des attributs), puis l'**exporte** vers un ou plusieurs backends.

C'est lui qui porte la portabilité : les apps ignorent la destination, seul le Collector la connaît.

## Pipeline

```
receivers → processors → exporters
```

- **receivers** : entrée (OTLP gRPC/HTTP…)
- **processors** : batch, filtrage, **tail sampling**, transformation
- **exporters** : sortie vers [[Application Insights]], [[Prometheus]], Tempo…

## Collector managé vs Collector complet

- Un **agent managé** (ex. [[Agent OpenTelemetry Azure Container Apps]]) est un Collector **bridé** : on choisit destinations + routage par signal, mais pas de pipeline arbitraire (pas de processors custom, pas de tail sampling).
- Pour du traitement avancé : **agent managé → Collector auto-hébergé (OTLP)**, où l'on pose la vraie pipeline, qui ensuite fan-out vers les backends.

## Voir aussi

- [[OpenTelemetry]]
- [[Agent OpenTelemetry Azure Container Apps]]
