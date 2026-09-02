
La **cardinalité** = le **nombre de combinaisons distinctes de labels** attachées à une [[Observabilité - métriques, logs et traces|métrique]]. C'est ce qui fait exploser — ou pas — la mémoire d'un backend comme [[Prometheus]].

Une métrique n'est pas une valeur unique : c'est une valeur **par combinaison de labels**. Chaque combinaison unique = une **série temporelle** stockée indépendamment.

## Exemple

`http_requests_total` avec :
- `method` (GET, POST, PUT, DELETE) → 4
- `status` (200, 404, 500…) → ~5
- `endpoint` (/users, /orders…) → ~20

Cardinalité = 4 × 5 × 20 = **400 séries**. Raisonnable.

Ajouter un label `user_id` (1 000 000 d'users) :

400 × 1 000 000 = **400 millions de séries** → Prometheus s'effondre (RAM saturée).

## La règle

- Un label doit avoir un ensemble de valeurs **borné et petit**.
- Les identifiants uniques (user_id, request_id, IP, timestamp) sont des **bombes à cardinalité** → interdits en label de métrique.
- Ces infos vont dans les **traces** ou les **logs**, conçus pour ça.

Sur un backend facturé au volume (ex. [[Application Insights]]), le risque n'est pas un crash mais un **coût** qui dérape.

## Voir aussi

- [[Observabilité - métriques, logs et traces]]
- [[Prometheus]]
