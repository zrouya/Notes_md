---
tags: [architecture, ddd, design-patterns]
---

# Ports (architecture hexagonale)

Un port est une interface définie **du point de vue du domaine** : elle exprime un besoin métier, jamais une technologie.

## Deux catégories

| Type | Sens de l'appel | Exemple |
|---|---|---|
| **Driven / outbound** (secondary) | Le domaine appelle vers l'extérieur | `IEnvelopeStore`, `IEmailSender` |
| **Driving / inbound** (primary) | L'extérieur déclenche le domaine | Controller HTTP, handler de queue, CLI |

## Driven port (le plus courant)

Le domaine définit le contrat, l'infrastructure l'implémente :
```csharp
// Côté domaine
public interface IEnvelopeStore
{
    Task<Envelope?> Find(Guid id);
}
```
Aucune mention d'EF Core, de SQL ou de Postgres dans cette interface.

## Driving port

Représente un cas d'usage exposé par le domaine/l'application ; souvent implicite en .NET (une méthode publique d'un service applicatif joue ce rôle), mais peut être explicité par une interface (`ICreateEnvelopeUseCase`) dans des architectures plus strictes (clean architecture, use-case driven).

## Règle pratique

Si un type technique (`DbContext`, `HttpClient`, `IConfiguration`...) apparaît dans la signature d'un port, ce n'est plus un port — c'est déjà un détail d'implémentation qui a fuité dans le domaine.

## Voir aussi

- [[Architecture Hexagonale (Ports et Adapters)]]
- [[Adapters (Architecture Hexagonale)]]
- [[Repository Pattern (DDD)]]
