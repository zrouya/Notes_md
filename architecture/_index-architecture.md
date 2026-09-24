---
tags: [index, moc, architecture]
---

# Index — Architecture

Patterns d'architecture applicative : Event Sourcing, CQRS, DDD, messaging et cohérence dans les systèmes distribués.

## Parcours de lecture conseillé

1. [[event-sourcing]] → 2. [[cqrs]] → 3. [[event-sourcing-et-cqrs]] → 4. [[projection-event-sourcing]] → 5. [[coherence-a-terme]]

## Notes

| Note | Description |
|------|-------------|
| [[event-sourcing]] | Note pivot : stocker les événements plutôt que l'état, avantages, pièges, cas d'usage |
| [[cqrs]] | Séparer écriture et lecture : origine (CQS), niveaux de mise en œuvre |
| [[event-sourcing-et-cqrs]] | Pourquoi ES a besoin de CQRS, ce qu'ES apporte à CQRS, règles et pièges |
| [[agregat-ddd]] | Décider / évoluer : l'agrégat en Event Sourcing, exemple C# |
| [[event-store]] | Store append-only : schéma SQL minimal, opérations, outils (EventStoreDB, Marten) |
| [[projection-event-sourcing]] | Modèles de lecture : inline / async / live, idempotence, checkpoint, rebuild |
| [[snapshot-event-sourcing]] | Accélérer la réhydratation, et quand c'est un signe d'agrégat mal découpé |
| [[verrouillage-optimiste]] | Concurrence par version attendue : SQL, EF Core, event store, ETag HTTP |
| [[coherence-a-terme]] | Eventual consistency : stratégies UI/API, read-your-writes |
| [[versioning-evenements]] | Faire évoluer le schéma des événements : upcasting, weak schema |
| [[crypto-shredding]] | RGPD dans un store immuable : détruire la clé plutôt que la donnée |
| [[outbox-pattern]] | Écriture + publication atomiques sans transaction distribuée |

## Voir aussi

- [[_index-poo|POO & Conception]]
- [[invalidation-event-driven]]
