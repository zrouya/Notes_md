---
tags: [angular, routing]
---

# Module Router Angular

Fournit le service `Router` et les directives (`routerLink`, `router-outlet`, `routerLinkActive`) à l'application.

## Deux façons de l'activer

```ts
// Application standalone (moderne)
bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes, withComponentInputBinding())]
});
```

```ts
// Application à NgModules (historique)
@NgModule({
  imports: [RouterModule.forRoot(routes)],   // forChild(routes) dans les modules lazy
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

## Features `provideRouter` utiles

| Feature | Effet |
|---|---|
| `withComponentInputBinding()` | Params / query params / data injectés en `input()` |
| `withHashLocation()` | URL en `/#/...` (pas de fallback serveur nécessaire) |
| `withPreloading(PreloadAllModules)` | Précharge les routes lazy en tâche de fond |
| `withViewTransitions()` | Animations de transition entre pages |
| `withInMemoryScrolling(...)` | Restauration de la position de scroll |

## Voir aussi

- [[routing-angular]]
- [[modules-angular]]
- [[routing-navigateur-history-api]]
