---
tags: [observability, metrics, logs, traces, concepts]
---

# Observabilité — métriques, logs et traces

Les **trois piliers de l'observabilité**. Ils répondent à des questions différentes et se complètent.

## Métriques — « Est-ce que ça va, et à quel point ? »

- Des **nombres agrégés dans le temps** : taux de requêtes, latence p95, % CPU, nombre d'erreurs.
- Compact, peu cher à stocker, idéal pour dashboards et alertes.
- Ne dit **pas pourquoi**. Attention à la [[cardinalite-metriques|cardinalité]].

## Logs — « Que s'est-il passé précisément à cet instant ? »

- Des **événements datés et discrets**, souvent textuels.
- Riches en détail mais **isolés** : un événement dans un service, sans lien intrinsèque.
- Parfait pour le debug fin, l'audit, les messages d'erreur.

## Traces — « Par où est passée cette requête à travers tous les services ? »

- Suivent **une requête de bout en bout** dans un système distribué.
- Composées de **spans** (une étape = un span), chacun avec sa durée.
- Reliées par un **trace_id** partagé (voir [[correlation-traces-distribuees]]).
- Indispensables en micro-services pour localiser un goulot.

## L'analogie

- **Métrique** = tableau de bord de la voiture → il y a un souci.
- **Trace** = GPS montrant le trajet et où ça ralentit → *où* c'est lent.
- **Log** = boîte noire enregistrant chaque événement → *quoi* s'est passé.

Ils se chaînent : une métrique déclenche l'alerte → une trace localise le service lent → les logs disent pourquoi. Corrélation via le **trace_id**, que [[opentelemetry]] facilite.

## Voir aussi

- [[cardinalite-metriques]]
- [[correlation-traces-distribuees]]
- [[opentelemetry]]
