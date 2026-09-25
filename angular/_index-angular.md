---
tags: [index, angular]
---

# Angular — Map of Content

## Fondamentaux / TypeScript

- [[angular-overview]] — Vue d'ensemble du framework : modules, composants, directives, services, routing, data binding, DI
- [[typescript]] — Sur-ensemble de JavaScript, langage principal d'Angular
- [[decorateurs-en-typescript]] — Syntaxe `@`, métadonnées, équivalent des attributs C#
- [[structure-de-fichiers-d-une-application-angular]] — Arborescence `src/`, assets, environments, `angular.json`
- [[angularjs]] — (à compléter)
- [[application-hybride-angularjs-angular]] — Coexistence AngularJS/Angular via `UpgradeModule`

## Modules & Services

- [[modules-angular]] — Conteneurs de composants/directives/services, `AppModule`, `@NgModule`
- [[services-angular]] — (à compléter)
- [[injection-de-dependances-en-angular]] — (à compléter)

## Composants

- [[composants-angular]] — Blocs de base d'une application Angular, anatomie d'un composant
- [[metadonnees-d-un-composant-angular]] — Décorateur `@Component`
- [[composants-angular-donnees-dynamiques]] — Interpolation, property/attribute/event/class binding
- [[angular-component-inputs-outputs]] — `@Input`/`@Output` (classique et syntaxe Signals)
- [[angular-signals]] — Gestion réactive de l'état depuis Angular 16
- [[state-managment-angular]] — Zone.js vs Signals
- [[angular-styles-scss]] — Styles de composant, encapsulation, `:host`, SCSS et `angular.json`

## Directives & Data Binding

- [[directives-angular]] — Les trois types de directives, directive personnalisée
- [[angular-ngif]] — Rendu conditionnel (`*ngIf` et `@if`)
- [[angular-ngfor]] — Itération sur une liste (`*ngFor` et `@for`)
- [[data-binding-angular]] — (à compléter)
- [[formulaires-angular]] — `FormsModule`

## Routing

- [[routing-angular]] — Vue d'ensemble : RouterModule, Routes, RouterOutlet, guards, lazy loading
- [[module-router-angular]] — (à compléter)
- [[routes-angular]] — Objet `Routes`, redirections, route joker
- [[configuration-des-routes-en-angular]] — (à compléter)
- [[routes-imbriquees-en-angular]] — (à compléter)
- [[routeroutlet-angular]] — Directive `router-outlet`
- [[routeroutlet-nommes]] — Outlets nommés pour vues multiples

## Rendu serveur (SSR)

- [[csr-ssr-ssg]] — Stratégies de rendu web : CSR, SSR, SSG, critères de choix
- [[angular-ssr]] — `@angular/ssr` : mise en place, fichiers générés, `server.ts`, build et exécution
- [[angular-hydratation]] — Hydratation, event replay, hydratation incrémentale `@defer`, transfer cache
- [[angular-ssr-render-modes]] — `RenderMode` Server / Prerender / Client par route
- [[angular-ssr-pieges]] — Code isomorphe : API navigateur, `afterNextRender`, auth, stabilité
