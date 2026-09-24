---
tags: [architecture, event-sourcing, performance]
---

# Snapshot (Event Sourcing)

Photo de l'état d'un agrégat à la version N, pour ne pas rejouer tout son flux à chaque chargement.

## Chargement

```
1. Lire le dernier snapshot     → état à la version 9 500
2. Lire le flux depuis 9 501    → 12 événements
3. Appliquer les 12 événements  → état courant
```

## Détails

- Stratégie courante : un snapshot tous les N événements (ex. 100 ou 1 000).
- Le snapshot est un **cache jetable** : on peut toujours le supprimer et tout rejouer.
- Si la structure de l'état change, on invalide les snapshots (version du schéma de snapshot).
- Avant d'en ajouter, se demander si l'agrégat n'est pas **mal découpé** : un flux de 10 000 événements signale souvent un agrégat trop gros (ex. découper par période : `compte-42-2026-09`).
- Charger quelques centaines d'événements est généralement rapide : ne pas optimiser trop tôt.

## Voir aussi

- [[event-sourcing]]
- [[agregat-ddd]]
