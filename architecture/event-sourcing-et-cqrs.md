---
tags: [architecture, event-sourcing, cqrs]
---

# Event Sourcing et CQRS : les liens

Deux patterns distincts mais complémentaires : l'Event Sourcing fournit un côté écriture qu'on ne peut pas interroger, et CQRS fournit le côté lecture qui lui manque. Les **événements** servent de pont entre les deux.

## Pourquoi l'Event Sourcing a besoin de CQRS

- Un event store ne sait lire **qu'un flux par son id** (`compte-42`).
- Impossible de faire `WHERE solde > 1000` ou une liste paginée → il faut des **projections**.
- Relation asymétrique : CQRS sans ES est courant, ES sans CQRS est quasi impossible.

## Ce que l'Event Sourcing apporte à CQRS

- **Synchronisation native** : sans ES, il faut publier un événement *en plus* de l'écriture en base (double écriture, [[outbox-pattern]]). Avec ES, l'événement **est** l'écriture : pas de double écriture, le store fait office d'outbox.
- **Rejouabilité** : une projection boguée ou un nouveau besoin se reconstruisent depuis l'événement n°1.
- **Plusieurs vues** peuvent être alimentées par le même flux, sans toucher au côté écriture.

## Répartition des rôles

| Côté écriture | Côté lecture |
|---|---|
| Commande → agrégat réhydraté | Requête → modèle de lecture |
| Valide les invariants | Aucune règle métier |
| Event store (append-only) | SQL, document, Elastic, Redis… |
| Cohérence forte (par flux) | [[coherence-a-terme]] (sauf projection inline) |

## Règles et pièges

- **Valider une commande avec l'agrégat**, jamais avec une projection (elle peut être en retard).
- Unicité transverse (email unique) : l'agrégat ne voit que son flux → table de réservation, ou projection + compensation.
- Commandes : retourner l'id + la **version** pour que le client puisse attendre la projection (*read-your-writes*).
- **Événements de domaine ≠ événements d'intégration** : ne pas exposer les événements internes aux autres services, publier un contrat stable dédié.
- **Sagas / process managers** : consomment des événements et émettent des commandes (orchestration longue).

## Voir aussi

- [[event-sourcing]]
- [[cqrs]]
- [[projection-event-sourcing]]
- [[agregat-ddd]]
