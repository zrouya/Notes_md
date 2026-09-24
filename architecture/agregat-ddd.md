---
tags: [architecture, ddd, event-sourcing, csharp]
---

# Agrégat (DDD) en Event Sourcing

Frontière de cohérence qui garde les invariants métier. En Event Sourcing, il **décide** (valide puis émet des événements) et **évolue** (applique les événements, sans aucune validation).

## Exemple C#

```csharp
public class Compte
{
    public decimal Solde { get; private set; }
    public int Version { get; private set; }
    private readonly List<EvenementCompte> _nonCommites = new();
    public IReadOnlyList<EvenementCompte> NonCommites => _nonCommites;

    public static Compte Depuis(IEnumerable<EvenementCompte> historique)
    {
        var c = new Compte();
        foreach (var e in historique) { c.Appliquer(e); c.Version++; }
        return c;
    }

    // Décision : valide, puis émet
    public void Retirer(decimal montant)
    {
        if (montant > Solde) throw new InvalidOperationException("Solde insuffisant");
        Emettre(new ArgentRetire(montant));
    }

    private void Emettre(EvenementCompte e) { Appliquer(e); _nonCommites.Add(e); }

    // Évolution : pure, ne lève jamais d'exception
    private void Appliquer(EvenementCompte e)
    {
        switch (e)
        {
            case ArgentDepose d: Solde += d.Montant; break;
            case ArgentRetire r: Solde -= r.Montant; break;
        }
    }
}
```

## Détails

- `Appliquer` ne valide **jamais** : un événement stocké est un fait, son rejeu ne doit pas échouer.
- L'agrégat ne garantit la cohérence que **dans son propre flux**.
- Agrégats petits = flux courts, moins de conflits de concurrence.
- Variante fonctionnelle : `decide(commande, état) → événements` et `evolve(état, événement) → état`.

## Voir aussi

- [[event-sourcing]]
- [[verrouillage-optimiste]]
- [[snapshot-event-sourcing]]
