---
tags: [angular, routing, fondamentaux]
---

# Routing Angular

Le Router associe **l'URL** au **composant affiché** dans une SPA, sans rechargement de page, tout en conservant le comportement du navigateur (historique, favoris, liens partageables, F5).

## Mise en place (standalone)

```ts
// app.routes.ts
export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'produits/:id', component: ProductDetailComponent },
  { path: '**', component: NotFoundComponent },
];

// main.ts
bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes, withComponentInputBinding())]
});
```

```html
<a routerLink="/produits" routerLinkActive="actif">Produits</a>
<router-outlet />
```

## Briques

| Élément | Rôle |
|---|---|
| `Routes` | Table URL → composant ([[configuration-des-routes-en-angular]]) |
| `provideRouter()` / `RouterModule.forRoot()` | Active le router ([[module-router-angular]]) |
| `<router-outlet>` | Zone d'affichage ([[routeroutlet-angular]]) |
| `routerLink` / `Router.navigate()` | Navigation sans rechargement |
| `ActivatedRoute` | Paramètres de la route active ([[parametres-de-route-angular]]) |
| Guards / resolvers | Autoriser, précharger ([[router-guards-angular]]) |
| `loadComponent` / `loadChildren` | Lazy loading |

## Cycle d'une navigation

1. Parsing de l'URL (chemin, params, query params, fragment)
2. **Matching** : première route qui correspond, dans l'ordre de déclaration
3. Redirections (`redirectTo`)
4. Guards (`canMatch`, `canActivate`, `canDeactivate`…)
5. Resolvers
6. Activation des composants dans les outlets + mise à jour de l'URL (`pushState`)

Événements observables via `router.events` : `NavigationStart`, `NavigationEnd`, `NavigationError`…

## Voir aussi

- [[routing-navigateur-history-api]]
- [[routes-angular]]
- [[routes-imbriquees-en-angular]]
- [[arbre-composants-vs-arbre-routes]]
