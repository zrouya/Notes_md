---
tags: [angular, routing, router-outlet]
---

# RouterOutlet nommés

Dans les scénarios les plus courants, il y a un [[routeroutlet-angular|router-outlet]] principal au niveau racine pour afficher la navigation principale basée sur les routes. Pour des situations plus complexes nécessitant plusieurs vues principales (vue latérale et vue principale par exemple), on utilise des outlets nommés : plusieurs `router-outlet` dans une seule vue, chacun avec un nom unique, ciblés individuellement par la configuration des routes.

## Exemple

```html
<!-- app.component.html -->
<router-outlet></router-outlet> <!-- outlet par défaut -->
<router-outlet name="sidebar"></router-outlet> <!-- outlet nommé 'sidebar' -->
```

[[configuration-des-routes-en-angular|Configuration des routes]] :

```js
const routes: Routes = [
  { path: 'accueil', component: AccueilComponent },
  { path: 'sidebar', component: SidebarComponent, outlet: 'sidebar' }
];
```

Avec cette configuration, naviguer vers `/accueil` affiche `AccueilComponent` dans l'outlet par défaut, et naviguer vers `/sidebar` affiche `SidebarComponent` dans l'outlet nommé « sidebar ».

## Voir aussi

- [[routeroutlet-angular]]
- [[configuration-des-routes-en-angular]]
