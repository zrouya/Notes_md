---
tags: [architecture, ddd, design-patterns]
---

# Adapters (architecture hexagonale)

Un adapter est une implémentation concrète d'un [[Ports (Architecture Hexagonale)|port]] : il traduit un besoin métier vers/depuis une technologie précise.

## Plusieurs adapters, un seul port

Le même port peut avoir plusieurs adapters interchangeables :

```csharp
public interface IEnvelopeStore { /* ... */ }

// Adapter "réel" — Infrastructure
public class EfEnvelopeStore : IEnvelopeStore
{
    private readonly DbContext _dbContext;
    // ... utilise EF Core / Postgres
}

// Adapter de test — Application.Tests
public class InMemoryEnvelopeStore : IEnvelopeStore
{
    private readonly ConcurrentDictionary<Guid, Envelope> _envelopes = new();
    // ... aucune dépendance technique externe
}
```

## Où vivent les adapters ?

- Les adapters **driven** (persistance, emails, API externes...) vivent dans la couche Infrastructure.
- Les adapters **driving** (controllers HTTP, consumers de queue...) vivent dans la couche présentation/API.
- Un adapter de test peut vivre à part (projet de tests) tant qu'il implémente le même port.

## Règle de dépendance

Un adapter dépend du port (et donc du domaine) — jamais l'inverse. Le projet d'infrastructure référence le projet domaine, mais le domaine ne référence jamais l'infrastructure.

## Voir aussi

- [[Architecture Hexagonale (Ports et Adapters)]]
- [[Ports (Architecture Hexagonale)]]
- [[Composition Root]]
