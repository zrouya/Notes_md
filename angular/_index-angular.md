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
- [[services-angular]] — `@Injectable`, portée (`providedIn: 'root'` = singleton), usages
- [[injection-de-dependances-en-angular]] — Principe (IoC), token/provider/injecteur, `inject()`, contexte d'injection
- [[angular-di-providers]] — `useClass`/`useValue`/`useFactory`/`useExisting`, `InjectionToken`, `multi`
- [[angular-di-hierarchie-injecteurs]] — Arbre d'injecteurs, résolution, portée, `optional`/`self`/`skipSelf`/`host`
- [[angular-di-tests-erreurs]] — Mocks avec TestBed, `NullInjectorError`, NG0203, dépendances circulaires

## Composants

- [[composants-angular]] — Blocs de base d'une application Angular, anatomie d'un composant
- [[metadonnees-d-un-composant-angular]] — Décorateur `@Component`
- [[composants-angular-donnees-dynamiques]] — Interpolation, property/attribute/event/class binding
- [[angular-component-inputs-outputs]] — `@Input`/`@Output` et `input()`/`output()`/`model()`
- [[arbre-composants-vs-arbre-routes]] — Composition par template vs routes enfants, composant racine
- [[angular-styles-scss]] — Styles de composant, encapsulation, `:host`, SCSS et `angular.json`

## Réactivité & état

- [[state-managment-angular]] — Zone.js vs Signals, mode zoneless
- [[angular-signals]] — `signal`, `computed`, `effect` : principes et usage
- [[angular-signals-avance]] — `linkedSignal`, `resource`/`httpResource`, `untracked`
- [[angular-signals-pieges]] — Mutation, effect mal utilisé, parenthèses, dépendances
- [[signals-vs-rxjs]] — Valeur vs flux d'événements, glitch, quand utiliser quoi
- [[rxjs-signals-interop]] — `toSignal`, `toObservable`, pattern recherche avec debounce
- [[etat-partage-service-vs-url]] — Service singleton + signaux vs query param / URL

## Directives & Data Binding

- [[directives-angular]] — Les trois types de directives, directive personnalisée
- [[angular-ngif]] — Rendu conditionnel (`*ngIf` et `@if`)
- [[angular-ngfor]] — Itération sur une liste (`*ngFor` et `@for`)
- [[data-binding-angular]] — (à compléter)
- [[formulaires-angular]] — `FormsModule`

## Routing

- [[routing-angular]] — Vue d'ensemble, mise en place standalone, cycle de navigation
- [[routing-navigateur-history-api]] — API History, `pushState`/`popstate`, F5, fallback serveur
- [[module-router-angular]] — `provideRouter` vs `RouterModule.forRoot`, features `with*`
- [[routes-angular]] — Objet `Routes`, redirections, route joker
- [[configuration-des-routes-en-angular]] — Propriétés d'une route, lazy loading, ordre, `pathMatch`
- [[routes-imbriquees-en-angular]] — `children`, layout parent + outlet enfant
- [[parametres-de-route-angular]] — `ActivatedRoute`, input binding, `navigate`, réutilisation
- [[router-guards-angular]] — Guards fonctionnels, `canMatch`/`canActivate`, resolvers
- [[routeroutlet-angular]] — Directive `router-outlet`
- [[routeroutlet-nommes]] — Outlets nommés, syntaxe d'URL, alternatives

## Rendu serveur (SSR)

- [[csr-ssr-ssg]] — Stratégies de rendu web : CSR, SSR, SSG, critères de choix
- [[angular-ssr]] — `@angular/ssr` : mise en place, fichiers générés, `server.ts`, build et exécution
- [[angular-hydratation]] — Hydratation, event replay, hydratation incrémentale `@defer`, transfer cache
- [[angular-ssr-render-modes]] — `RenderMode` Server / Prerender / Client par route
- [[angular-ssr-pieges]] — Code isomorphe : API navigateur, `afterNextRender`, auth, stabilité
