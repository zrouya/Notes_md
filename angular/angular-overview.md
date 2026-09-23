---
tags: [angular, framework, fondamentaux]
---

# Angular

Framework de développement d'applications web front-end, développé par Google et écrit en [[typescript|TypeScript]]. Il structure une application autour de modules, composants, directives et services, et fournit un système de routage et de data binding intégré.

## Concepts clés

1. **[[modules-angular|Modules]]** : conteneurs pour différentes parties de l'application (composants, directives, services). Le module racine est `AppModule`.
2. **[[composants-angular|Composants]]** : blocs de base d'une application Angular, chacun contrôlant une partie de l'écran (une vue).
3. **[[directives-angular|Directives]]** : fonctions qui modifient le DOM ou le comportement d'un élément DOM (composants, directives structurelles, directives d'attributs).
4. **[[services-angular|Services]]** : classes encapsulant la logique métier et les données, partageables entre composants.
5. **[[routing-angular|Routing]]** : navigation entre les différentes parties de l'application, basée sur l'URL.
6. **[[data-binding-angular|Data Binding]]** : synchronisation des données entre le modèle et la vue (one-way ou two-way binding).
7. **[[injection-de-dependances-en-angular|Injection de dépendances]]** : mécanisme de création et de gestion des objets de l'application.

## Structure de fichiers

Voir [[structure-de-fichiers-d-une-application-angular|Structure de fichiers d'une application Angular]] pour le détail de l'arborescence `src/`, des assets, environnements et du fichier `angular.json`.

## CLI et lancement

- `ng new` : création d'une nouvelle application.
- `ng serve` : lancement en mode développement (serveur de développement + ouverture navigateur).
- `ng build` : construction de l'application pour la production.

## Mécanismes sous-jacents

- Un mécanisme de **détection des changements** vérifie les modifications des données et met à jour la vue en conséquence.
- Un mécanisme d'**injection de dépendances** rend l'application plus modulaire et plus facile à tester.

## Voir aussi

- [[typescript]]
- [[modules-angular]]
- [[composants-angular]]
- [[routing-angular]]
- [[angular-signals]] — mécanisme alternatif de gestion d'état/détection de changements depuis Angular 16
