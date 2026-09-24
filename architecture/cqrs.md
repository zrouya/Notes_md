---
tags: [architecture, cqrs]
---

# CQRS (Command Query Responsibility Segregation)

Séparer le **modèle d'écriture** (commandes, qui modifient) du **modèle de lecture** (requêtes, qui lisent). Chacun est optimisé pour son usage.

## Origine

- **CQS** (Bertrand Meyer) : au niveau d'une *méthode*, soit elle modifie l'état, soit elle retourne une valeur, jamais les deux.
- **CQRS** (Greg Young, ~2010) : le même principe appliqué au niveau de l'*architecture*, avec deux modèles distincts.

## Schéma

```
          Commande                         Requête
             │                                │
             ▼                                ▼
   Modèle d'écriture                 Modèle de lecture
   (règles métier, agrégats)         (DTO plats, dénormalisés)
             │                                ▲
             └──── synchronisation ───────────┘
```

## Niveaux de mise en œuvre

| Niveau | Description |
|---|---|
| 1. Code séparé | Même base, handlers de commande / de requête distincts (ex. MediatR) |
| 2. Modèles séparés | Même base, mais vues SQL ou tables de lecture dédiées |
| 3. Stores séparés | Base d'écriture ≠ base(s) de lecture (SQL + Elastic, Redis…) |
| 4. Avec Event Sourcing | L'écriture est un event store, les lectures sont des projections |

## Détails

- Une commande retourne **rien**, ou juste un id / une version, jamais un modèle de lecture.
- Les requêtes ne passent jamais par le modèle métier : elles lisent des DTO prêts à afficher.
- On peut avoir **plusieurs modèles de lecture** pour une même écriture (liste, recherche, reporting).
- Dès que les stores sont séparés : [[coherence-a-terme]].
- **CQRS n'exige pas l'Event Sourcing**, mais l'Event Sourcing exige presque toujours CQRS.

## Voir aussi

- [[event-sourcing-et-cqrs]]
- [[event-sourcing]]
- [[projection-event-sourcing]]
