---
tags: [angular, routing, securite]
---

# Guards et resolvers Angular

Fonctions exécutées par le Router **pendant la navigation**, avant l'affichage du composant.

## Guard fonctionnel

```ts
export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  return auth.isLoggedIn()
    ? true
    : inject(Router).createUrlTree(['/login']);   // redirection
};

{ path: 'admin', canActivate: [authGuard], loadComponent: () => import('./admin.component').then(m => m.AdminComponent) }
```

Valeur de retour : `true` (autorisé), `false` (annulé) ou une `UrlTree` (redirection). Peut aussi être un `Observable` ou une `Promise`.

## Types de guards

| Guard | Quand |
|---|---|
| `canMatch` | Avant le matching : la route est ignorée si `false` (le code lazy n'est pas chargé) |
| `canActivate` | Avant d'activer la route |
| `canActivateChild` | Avant d'activer une route enfant |
| `canDeactivate` | Avant de quitter (ex. « modifications non sauvegardées ») |

## Resolver

Précharge des données avant d'afficher le composant :

```ts
export const produitResolver: ResolveFn<Produit> = route =>
  inject(ProduitService).get(route.paramMap.get('id')!);

{ path: 'produits/:id', component: DetailComponent, resolve: { produit: produitResolver } }
```

Les guards côté client améliorent l'UX mais **ne sécurisent rien** : l'API doit toujours vérifier les droits.

## Voir aussi

- [[routing-angular]]
- [[configuration-des-routes-en-angular]]
