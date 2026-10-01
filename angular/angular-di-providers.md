---
tags: [angular, injection-dependances]
---

# Providers Angular

Un provider indique à l'injecteur **comment obtenir la valeur** associée à un token.

## `providedIn: 'root'` (cas courant)

```ts
@Injectable({ providedIn: 'root' })
export class ProduitService {}
```

Singleton applicatif et **tree-shakable** : retiré du bundle si personne ne l'injecte.

## Types de recettes

```ts
providers: [
  ProduitService,                                        // = { provide: ProduitService, useClass: ProduitService }
  { provide: Logger, useClass: ConsoleLogger },          // substituer une implémentation
  { provide: API_URL, useValue: 'https://api.ex.com' },  // valeur fixe
  { provide: Storage, useFactory: () =>                  // fabrication (peut utiliser inject())
      isBrowser() ? localStorage : new MemoryStorage() },
  { provide: AncienLogger, useExisting: Logger },        // alias vers un autre token
]
```

## `InjectionToken`

Pour injecter ce qui n'est pas une classe (chaîne, config, interface — inexistante à l'exécution) :

```ts
export const API_URL = new InjectionToken<string>('API_URL');

bootstrapApplication(AppComponent, {
  providers: [{ provide: API_URL, useValue: environment.apiUrl }]
});

private apiUrl = inject(API_URL);
```

## Multi providers

Plusieurs providers pour un même token → `inject()` renvoie un **tableau**. Utilisé pour les intercepteurs, validateurs, initialiseurs.

```ts
{ provide: PLUGINS, useClass: PluginA, multi: true },
{ provide: PLUGINS, useClass: PluginB, multi: true },
// inject(PLUGINS) → [PluginA, PluginB]
```

## Voir aussi

- [[injection-de-dependances-en-angular]]
- [[angular-di-hierarchie-injecteurs]]
