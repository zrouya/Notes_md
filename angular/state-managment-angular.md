---
tags: [angular, state-management, signals]
---

# State management Angular

Avant la version 16 d'Angular, la gestion de l'état des composants était assurée par Zone.js : ce package détermine des « zones » autour des composants de l'application, représentant le périmètre à mettre à jour (donc les éléments du DOM associés) lors d'une modification de données.

À partir de la version 16 d'Angular (stabilisé en version 17), ce système peut être remplacé par les [[angular-signals|Signals]], un système basé sur une gestion événementielle (souscription aux événements de modification des données).

## Voir aussi

- [[angular-signals]]
- [[composants-angular-donnees-dynamiques]]
