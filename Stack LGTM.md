
La **stack LGTM** est l'ensemble open-source et **agnostique du fournisseur** de Grafana Labs couvrant les 3 piliers de l'[[Observabilité - métriques, logs et traces|observabilité]].

## Les composants

| Lettre | Outil | Pilier |
|---|---|---|
| **L** | Loki | Logs |
| **G** | [[Grafana]] | Visualisation |
| **T** | [[Grafana Tempo\|Tempo]] | Traces |
| **M** | Mimir | Métriques (Prometheus scalable) |

## Positionnement

- Pendant open-source complet des backends propriétaires (App Insights, Datadog…).
- Alimenté via [[OpenTelemetry]] (les apps émettent en OTLP vers un [[OpenTelemetry Collector|Collector]] qui fan-out vers ces backends).
- **Self-hosté** (contrôle du coût, ops à porter) ou **Grafana Cloud** (managé).

Choix cohérent quand la **portabilité** et le **multi-cloud/hybride** priment (voir [[Vendor lock-in]]).

## Voir aussi

- [[Grafana Tempo]]
- [[OpenTelemetry Collector]]
