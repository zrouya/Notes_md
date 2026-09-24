---
tags: [architecture, cqrs, systemes-distribues]
---

# Cohérence à terme (Eventual Consistency)

Après une écriture, les lectures peuvent renvoyer des données **en retard** pendant un court moment, mais finissent par converger. C'est la conséquence directe de modèles de lecture mis à jour de façon asynchrone.

## Exemple

```
t0  POST /comptes/42/retraits      → 202, { version: 8 }
t1  GET  /comptes/42 (projection)  → solde encore à la v7 ❌
t2  projection traite l'événement  → solde à jour ✅
```

## Stratégies côté client / API

| Stratégie | Principe |
|---|---|
| UI optimiste | Afficher le résultat attendu sans attendre la projection |
| Read-your-writes | La commande renvoie la version ; la requête attend que la projection l'ait atteinte |
| Projection inline | Mise à jour dans la même transaction (cohérence forte, coût en écriture) |
| Retour direct | La commande renvoie les données calculées par l'agrégat |
| Notification | Push (SignalR/WebSocket) quand la projection est à jour |

## Détails

- Le délai est généralement de quelques ms, mais peut grimper en cas d'incident : le surveiller (retard des projections).
- Ne **jamais** valider une règle métier sur des données de lecture potentiellement en retard.
- Souvent acceptable côté métier : le monde réel est lui-même à cohérence à terme (courrier, virements).

## Voir aussi

- [[cqrs]]
- [[projection-event-sourcing]]
- [[event-sourcing-et-cqrs]]
