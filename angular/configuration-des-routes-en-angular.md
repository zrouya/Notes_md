---
tags: [angular, routing]
---

# Configuration des routes en Angular

Propriétés principales d'un objet `Route` dans le tableau [[routes-angular|Routes]].

## Exemple

```ts
export const routes: Routes = [
  { path: '', redirectTo: 'accueil', pathMatch: 'full' },
  { path: 'accueil', component: AccueilComponent, title: 'Accueil' },
  { path: 'produits/:id', component: ProductDetailComponent },
  { path: 'profil', loadComponent: () =>
      import('./profil.component').then(m => m.ProfilComponent) },
  { path: 'admin', canActivate: [authGuard], loadChildren: () =>
      import('./admin/admin.routes').then(m => m.ADMIN_ROUTES) },
  { path: 'chat/:id', component: ChatComponent, outlet: 'panneau' },
  { path: '**', component: NotFoundComponent },
];
```

## Propriétés

| Propriété | Rôle |
|---|---|
| `path` | Segment d'URL (`:id` = paramètre, `**` = joker) |
| `component` | Composant chargé immédiatement |
| `loadComponent` / `loadChildren` | Lazy loading (chunk JS séparé) |
| `redirectTo` + `pathMatch` | Redirection |
| `children` | [[routes-imbriquees-en-angular]] |
| `canActivate`, `canMatch`, `canDeactivate`, `resolve` | [[router-guards-angular]] |
| `data`, `title` | Données statiques, titre de l'onglet |
| `outlet` | Cible un [[routeroutlet-nommes\|outlet nommé]] |

## Pièges

- **Ordre** : la première route qui correspond gagne → routes spécifiques avant génériques, `**` en dernier.
- **`pathMatch: 'full'`** obligatoire pour une redirection depuis `''` (sinon `''` est préfixe de toutes les URL).

## Voir aussi

- [[routing-angular]]
- [[routes-angular]]
