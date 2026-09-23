---
tags: [architecture, dependency-injection, design-patterns]
---

# Composition Root

Le point unique de l'application où les [[Ports (Architecture Hexagonale)|ports]] sont associés à leurs [[Adapters (Architecture Hexagonale)|adapters]] concrets, via l'injection de dépendances.

## Rôle

C'est le seul endroit du code autorisé à connaître **à la fois** l'abstraction (port) et son implémentation (adapter). Tout le reste du code ne dépend que du port.

```csharp
// Program.cs — composition root d'une app ASP.NET Core
builder.Services.AddDbContext<CamuDbContext>(options =>
    options.UseNpgsql(connectionString));

builder.Services.AddScoped<IEnvelopeStore, EfEnvelopeStore>();
```

## Pourquoi ça compte

- Sans composition root, le choix de l'implémentation se disperse dans le code (`new EfEnvelopeStore(...)` un peu partout) → couplage fort, impossible à substituer en test.
- En centralisant le câblage, changer d'adapter (ex : remplacer `EfEnvelopeStore` par un autre store) ne touche qu'un seul fichier.

## En test

Un projet de tests d'intégration peut définir son propre composition root partiel, qui remplace certains adapters (ex : `WebApplicationFactory` + `ConfigureServices` qui substitue l'adapter réel par un adapter in-memory).

## Voir aussi

- [[Architecture Hexagonale (Ports et Adapters)]]
- [[Adapters (Architecture Hexagonale)]]
