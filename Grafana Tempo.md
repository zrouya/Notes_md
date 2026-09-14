
**Grafana Tempo** est un **backend de tracing distribué** open-source (Grafana Labs) : le **magasin de traces**, agnostique du fournisseur.

## Fonctionnement

```
Apps / Collector ──OTLP──► Tempo ──► stockage objet (Azure Blob / S3)
                                          │
                                       Grafana ──TraceQL──► waterfall
```

- **Ingestion** : reçoit les spans en **OTLP** (protocole [[OpenTelemetry]] natif).
- **Stockage** : gros volumes à **bas coût** sur du stockage objet, index minimal (`trace_id` + langage **TraceQL**).
- **Restitution** : [[Grafana]] rend le waterfall, avec corrélation vers logs/métriques via le `trace_id`.

## vs Application Insights

Les deux **stockent des traces**. Différence :
- **[[Application Insights]]** : APM managé Azure, KQL, facturé au Go, lock-in Azure.
- **Tempo** : self-hosté (ou Grafana Cloud), **OTLP natif**, stockage objet bon marché, **neutre**.

Même instrumentation OTel dans les deux cas : changer de backend ne touche pas le code (voir [[Vendor lock-in]]).

## Voir aussi

- [[Stack LGTM]]
- [[OpenTelemetry Collector]]
- [[Corrélation de traces distribuées]]
