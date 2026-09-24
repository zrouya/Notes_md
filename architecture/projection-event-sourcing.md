---
tags: [architecture, event-sourcing, cqrs]
---

# Projection (Event Sourcing)

Composant qui consomme les événements et construit un **modèle de lecture** optimisé pour les requêtes. C'est le côté lecture de [[cqrs]].

## Exemple

```csharp
public class SoldeProjection
{
    public async Task Handle(EventEnvelope env)
    {
        if (env.Position <= await _checkpoint.Get()) return;   // idempotence
        switch (env.Event)
        {
            case ArgentDepose d: await _db.Exec("UPDATE soldes SET solde = solde + @m WHERE id = @id", ...); break;
            case ArgentRetire r: await _db.Exec("UPDATE soldes SET solde = solde - @m WHERE id = @id", ...); break;
        }
        await _checkpoint.Save(env.Position);  // idéalement dans la même transaction
    }
}
```

## Types de projection

| Type | Fonctionnement | Cohérence |
|---|---|---|
| Inline | Mise à jour dans la même transaction que l'ajout | Forte, mais écriture ralentie |
| Asynchrone | Démon abonné au flux global, avec checkpoint | [[coherence-a-terme]] |
| Live | Calculée à la volée à la lecture | Forte, coûteuse |

## Règles

- **Idempotente** : livraison *at-least-once*, un événement peut arriver deux fois.
- **Checkpoint** : mémoriser la dernière position traitée.
- **Reconstructible** : pouvoir tout effacer et rejouer depuis le début (blue/green : on construit la v2 à côté, puis on bascule).
- Aucune règle métier : une projection ne refuse jamais un événement.
- Une projection par besoin de lecture (liste, recherche, reporting…).

## Voir aussi

- [[event-sourcing-et-cqrs]]
- [[event-store]]
- [[invalidation-event-driven]]
