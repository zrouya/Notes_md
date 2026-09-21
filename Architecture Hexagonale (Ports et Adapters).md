---
tags: [architecture, ddd, dotnet, design-patterns]
---

# Architecture Hexagonale (Ports et Adapters)

Pattern d'architecture (Alistair Cockburn) qui isole le domaine métier de toute dépendance technique, en forçant le sens des dépendances à pointer vers l'intérieur.

## Idée centrale

Le domaine ne doit jamais dépendre d'un détail technique (base de données, framework web, message broker...). Pour ça :
- Le domaine définit des **[[Ports (Architecture Hexagonale)|ports]]** : des interfaces exprimées en vocabulaire métier.
- Les technologies concrètes sont implémentées comme des **[[Adapters (Architecture Hexagonale)|adapters]]** de ces ports, à l'extérieur du domaine.
- Seul le **[[Composition Root]]** connaît à la fois les ports et leurs adapters, pour faire le câblage (injection de dépendances).

C'est une application directe du principe d'inversion de dépendance (le *D* de SOLID) : les dépendances pointent vers l'intérieur (le domaine), jamais l'inverse.

## Schéma

```
     Driving adapters                  Driven adapters
  (HTTP controller, CLI...)        (EF repository, SMTP...)
          │                                  ▲
          ▼                                  │
   ┌───────────────────────────────────────────────┐
   │                    DOMAINE                     │
   │         (ports = interfaces métier)             │
   └───────────────────────────────────────────────┘
```

## Exemple (.NET)

Port défini côté domaine :
```csharp
public interface IEnvelopeStore
{
    Task<Envelope?> Find(Guid id);
    Task Add(Envelope envelope);
}
```

Deux adapters différents du même port :
- `EfEnvelopeStore` → implémentation Postgres/EF Core, dans la couche Infrastructure
- `InMemoryEnvelopeStore` → double de test, dans la couche Application/Tests

## Bénéfices

- **Testabilité** : on teste le comportement métier réel en substituant un adapter in-memory à l'adapter réel.
- **Remplaçabilité** : changer de techno de persistance = écrire un nouvel adapter, sans toucher au domaine.
- **Dépendances protégées** : impossible qu'un détail technique (`DbContext`, `SaveChanges`...) fuite dans la logique métier.

## Voir aussi

- [[Ports (Architecture Hexagonale)]]
- [[Adapters (Architecture Hexagonale)]]
- [[Composition Root]]
- [[Repository Pattern (DDD)]]
