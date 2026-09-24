---
tags: [architecture, event-sourcing, versioning]
---

# Versioning des événements (upcasting)

Les événements stockés sont immuables, mais leur schéma évolue. On ne réécrit **jamais** l'historique : on adapte la lecture.

## Upcasting

Convertir à la lecture un ancien format vers le format courant, avant qu'il n'atteigne l'agrégat ou les projections.

```csharp
public record ArgentDeposeV1(decimal Montant);
public record ArgentDepose(decimal Montant, string Devise);

public static object Upcast(object e) => e switch
{
    ArgentDeposeV1 v1 => new ArgentDepose(v1.Montant, "EUR"),  // valeur par défaut historique
    _ => e
};
```

## Stratégies par type de changement

| Changement | Approche |
|---|---|
| Ajout de champ optionnel | Désérialisation tolérante, valeur par défaut |
| Renommage / restructuration | Upcaster V1 → V2 |
| Changement de sens métier | **Nouvel événement**, pas une nouvelle version |
| Nettoyage massif | Copy-and-transform vers un nouveau store (dernier recours) |

## Détails

- Stocker le **type + version** dans les métadonnées de chaque événement.
- Désérialisation *weak schema* : ignorer les champs inconnus, tolérer les manquants.
- Les upcasters s'enchaînent (V1 → V2 → V3) et doivent être testés.

## Voir aussi

- [[event-sourcing]]
- [[event-store]]
