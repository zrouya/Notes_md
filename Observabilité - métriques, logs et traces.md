
Les **trois piliers de l'observabilité**. Ils répondent à des questions différentes et se complètent.

## Métriques — « Est-ce que ça va, et à quel point ? »

- Des **nombres agrégés dans le temps** : taux de requêtes, latence p95, % CPU, nombre d'erreurs.
- Compact, peu cher à stocker, idéal pour dashboards et alertes.
- Ne dit **pas pourquoi** — juste que quelque chose bouge.
- Attention à la [[Cardinalité des métriques|cardinalité]].

## Logs — « Que s'est-il passé précisément à cet instant ? »

- Des **événements datés et discrets**, souvent textuels : `14:32:07 ERROR paiement refusé user=1234`.
- Riches en détail mais **isolés** : décrivent un événement dans un service, sans lien intrinsèque.
- Parfait pour le debug fin, l'audit, les messages d'erreur.

## Traces — « Par où est passée cette requête à travers tous les services ? »

- Suivent **une requête de bout en bout** dans un système distribué.
- Composées de **spans** (une étape = un span : APIM → Container App → SQL…), chacun avec sa durée.
- Tous les spans d'une requête partagent un **trace_id** → permet de dire « 3 s au total, dont 2,8 s dans l'appel SQL ».
- Indispensables en **micro-services** pour localiser un goulot ou une panne.

## L'analogie

- **Métrique** = tableau de bord de la voiture → il y a un souci.
- **Trace** = GPS montrant le trajet et où ça ralentit → *où* c'est lent.
- **Log** = boîte noire enregistrant chaque événement → *quoi* exactement s'est passé.

Ils se chaînent : une métrique déclenche l'alerte → une trace localise le service lent → les logs disent pourquoi. La corrélation logs↔traces se fait via le **trace_id**, que [[OpenTelemetry]] facilite.

## Voir aussi

- [[Cardinalité des métriques]]
- [[OpenTelemetry]]
