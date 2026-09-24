---
tags: [architecture, event-sourcing, ddd]
---

# Event Sourcing

On stocke la **suite des événements** qui ont conduit à un état, pas l'état courant. L'état devient une vue dérivée, recalculée en rejouant les événements.

## Exemple

```
CompteOuvert   { Id: 42, Titulaire: "Zaki" }
ArgentDéposé   { Montant: 200 }
ArgentRetiré   { Montant: 50 }
               → Solde = 150 (recalculé, jamais stocké)
```

Analogies : relevé bancaire (le solde découle des opérations), Git (les commits sont la vérité, l'arbre de travail en découle).

## Cycle

1. **Charger** le flux de l'agrégat et le rejouer (réhydratation).
2. **Décider** : l'agrégat valide la commande et émet des événements.
3. **Ajouter** au store avec la version attendue ([[verrouillage-optimiste]]).
4. **Projeter** : les projections mettent à jour les modèles de lecture.

## Vocabulaire

| Concept | Rôle |
|---|---|
| Événement | Fait passé, immuable, métier, nommé au passé (`CommandePassée`) |
| Commande | Intention, peut être refusée (`PasserCommande`) |
| Agrégat | Garde les invariants, produit les événements → [[agregat-ddd]] |
| Event Store | Base append-only, un flux par agrégat → [[event-store]] |
| Projection | Événements → modèle de lecture → [[projection-event-sourcing]] |
| Snapshot | État à la version N pour accélérer le chargement → [[snapshot-event-sourcing]] |

## Avantages

- Audit complet gratuit, voyage dans le temps (« état au 31/12 »).
- Nouvelles vues créées **après coup** en rejouant tout l'historique.
- Débogage par rejeu, intégration naturelle entre services.
- Modèle proche du langage métier (Event Storming).

## Inconvénients

- Complexité : store, projections, snapshots, [[coherence-a-terme]].
- Évolution du schéma des événements → [[versioning-evenements]].
- RGPD difficile sur un store immuable → [[crypto-shredding]].
- Piège : événements techniques (`ChampModifié`) au lieu d'événements métier.

## Quand l'utiliser

- ✅ Finance, compta, logistique, assurance, audit légal, workflows complexes.
- ❌ CRUD simple, référentiels, besoin de lectures immédiatement cohérentes.
- Peut s'appliquer à **un seul bounded context**, pas à tout le système.

## Voir aussi

- [[cqrs]]
- [[event-sourcing-et-cqrs]]
- [[invalidation-event-driven]]
