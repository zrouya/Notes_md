---
tags: [architecture, messaging, systemes-distribues, cqrs]
---

# Outbox pattern

Garantir qu'une écriture en base et la publication d'un message se font **ensemble ou pas du tout**, sans transaction distribuée.

## Le problème : la double écriture

```csharp
await db.SaveChangesAsync();        // ✅ commit
await bus.Publish(new CommandePassee(id));  // 💥 crash → événement perdu
```

## Solution

```
Transaction locale :
  INSERT INTO commandes ...
  INSERT INTO outbox (type, payload)       ← même transaction

Relais (worker) :
  SELECT * FROM outbox WHERE sent = false
  → publie sur le bus → UPDATE outbox SET sent = true
```

## Détails

- Livraison *at-least-once* : les consommateurs doivent être **idempotents** (inbox pattern côté réception).
- Alternative au polling : CDC (Change Data Capture, ex. Debezium) sur la table outbox.
- Outils .NET : MassTransit, NServiceBus, Wolverine, CAP proposent une outbox intégrée.
- En [[event-sourcing]], l'event store **est** l'outbox : l'événement est l'écriture, les abonnés le lisent directement.

## Voir aussi

- [[event-sourcing-et-cqrs]]
- [[invalidation-event-driven]]
