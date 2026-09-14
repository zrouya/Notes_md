---
tags: [observability, metrics, cardinality, prometheus]
---

# Cardinalité des métriques

La **cardinalité** = le **nombre de combinaisons distinctes de labels** attachées à une métrique. C'est ce qui fait exploser — ou pas — la mémoire d'un backend comme Prometheus.

Une métrique n'est pas une valeur unique : c'est une valeur **par combinaison de labels**. Chaque combinaison = une **série temporelle** stockée indépendamment.

## Exemple

`http_requests_total` avec `method` (4) × `status` (5) × `endpoint` (20) = **400 séries**. Raisonnable.

Ajouter `user_id` (1 000 000) : 400 × 1 000 000 = **400 millions de séries** → effondrement (RAM saturée).

## La règle

- Un label doit avoir un ensemble de valeurs **borné et petit**.
- Les identifiants uniques (user_id, request_id, IP, timestamp) sont des **bombes à cardinalité** → interdits en label.
- Ces infos vont dans les **traces** ou les **logs**.

Sur un backend facturé au volume (ex. [[appinsights-principe|App Insights]]), le risque n'est pas un crash mais un **coût** qui dérape (voir [[log-analytics-cout]]).

## Voir aussi

- [[observabilite-piliers]]
- [[log-analytics-cout]]
