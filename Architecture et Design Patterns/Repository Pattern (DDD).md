---
tags: [ddd, architecture, design-patterns]
---

# Repository Pattern (DDD)

En Domain-Driven Design (Eric Evans), le repository donne l'illusion d'une collection en mémoire d'agrégats, en cachant la persistance réelle.

## Où vit quoi

| Élément | Couche |
|---|---|
| Interface du repository (le contrat) | **Domaine** — c'est un [[Ports (Architecture Hexagonale)\|port]] |
| Implémentation du repository (SQL, EF Core...) | **Infrastructure** — c'est un [[Adapters (Architecture Hexagonale)\|adapter]] |

```csharp
// Domaine : exprimé en vocabulaire métier, un agrégat à la fois
public interface IEnvelopeStore
{
    Task<Envelope?> Find(Guid id);
    Task Add(Envelope envelope);
}
```

Un repository porte toujours sur un **agrégat** dans son ensemble, jamais sur une table ou une entité interne à l'agrégat.

## Et la Factory ?

Même logique : la **factory** encapsule la logique de construction d'un agrégat quand elle est trop complexe pour un simple constructeur (invariants à poser dès la création, choix entre plusieurs façons de construire l'objet...). Comme le repository, elle reste un concept du **domaine** — jamais dans l'infrastructure.

## Lien avec l'architecture hexagonale

Le repository DDD est un cas particulier de [[Ports (Architecture Hexagonale)|port driven]] : le domaine exprime le besoin (« retrouver un agrégat »), l'infrastructure fournit l'implémentation technique.

## Voir aussi

- [[Architecture Hexagonale (Ports et Adapters)]]
- [[Ports (Architecture Hexagonale)]]
