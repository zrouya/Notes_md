---
tags: [observability, poc, architecture, azure, moc]
---

# POC Observabilité SI hybride

Synthèse de la stratégie d'un POC d'observabilité extensible à un **SI hybride** (Azure + OnPrem), **multi-stack** (.Net, Java, PHP). Sert de point d'entrée vers les notes du sujet.

## Décision structurante

- **[[opentelemetry|OpenTelemetry]] = fondation non négociable.** Instrumenter une fois, corréler par `trace_id` quel que soit le langage. La vraie décision restante = le **backend** et **où l'héberger**.
- **Backend neutre self-hosté dans Azure** : périmètre maîtrisé, pas d'egress vers un SaaS externe. [[vendor-lock-in|Lock-in]] faible sur la partie coûteuse (le code).
- **L'APIM comme point d'appui** : tous les appels inter-services y transitent (OnPrem inclus) → point d'observation unique déjà sous contrôle.

## Architecture en couches

| Couche | Quoi | Dépendance | Apport |
|---|---|---|---|
| **0 — Gateway** | Diagnostic APIM → [[log-analytics\|Log Analytics]] → [[grafana\|Grafana]] | Aucune | **Boîte noire** de tout le SI |
| **1 — Apps Azure** | [[opentelemetry\|OTel]] sur les apps contrôlées, traces propagées via APIM | Aucune | **Boîte blanche** : *pourquoi* |
| **2 — OnPrem profond** | Instrumentation apps OnPrem + métriques infra | Prestataire | Différée |

Principe : **montrer d'abord (couches 0-1 autonomes), faire valider ensuite**.

## Couche 0 — plan concret

- Diagnostic APIM ([[diagnostic-settings]]) → catégorie `GatewayLogs` → table [[apim-gateway-logs]]. Sampling 100 %, pas de bodies/headers.
- Grafana en Container App (Managed Identity + rôle `Monitoring Reader`), datasource Azure Monitor.
- 4 dashboards : vue d'ensemble · RED par API · latence gateway-vs-backend · erreurs & top offenders.
- Attention au [[log-analytics-cout|coût d'ingestion]] (retirer `AllMetrics` si besoin).

## Couche 1 — instrumentation

- **Java** : `-javaagent` (zéro-code). **.Net** : distro OTel / auto-instrumentation.
- Propagation **W3C `traceparent`** via l'APIM ([[correlation-traces-distribuees]]) → recoud couche 0 + couche 1.
- Traces lisibles dans Grafana depuis [[appinsights-tables-requests-dependencies|App Insights]] d'abord, puis [[grafana-tempo|Tempo]] pour le backend neutre.

## Cas des brokers (Azure Service Bus)

Propagation native du `traceparent` dans les *application properties*. Le hop async « claque » en démo. Lag = métrique, pas trace.

## Candidat POC idéal

`.Net (Azure, instrumenté) → APIM → Azure Service Bus → Java (OnPrem, javaagent) → backend`
→ cross-stack, cross-frontière, sync **et** async, gateway + spans internes.

## Voir aussi

**Fondations** : [[opentelemetry]] · [[opentelemetry-collector]] · [[container-apps-otel-agent]] · [[observabilite-piliers]] · [[correlation-traces-distribuees]] · [[cardinalite-metriques]] · [[vendor-lock-in]]
**Briques Azure** : [[log-analytics]] · [[appinsights-principe]] · [[apm]] · [[diagnostic-settings]] · [[apim-gateway-logs]] · [[appinsights-tables-requests-dependencies]] · [[log-analytics-cout]]
**Visualisation** : [[grafana]] · [[grafana-tempo]] · [[stack-lgtm]]
