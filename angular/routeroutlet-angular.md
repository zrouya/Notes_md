---
tags: [angular, routing, router-outlet]
---

# RouterOutlet Angular

La directive `router-outlet` est un élément fondamental du système de [[routing-angular|routage d'Angular]]. Elle sert de marqueur ou de point d'ancrage dans les templates où le routeur doit afficher le composant correspondant à la route activée. Quand on navigue d'une route à une autre, le routeur détecte le composant à afficher et le place dans le `router-outlet`.

## Exemple

```html
<!-- app.component.html -->
<nav>
  <a routerLink="/accueil">Accueil</a>
  <a routerLink="/details">Détails</a>
</nav>

<router-outlet></router-outlet>
```

Avec une configuration de [[routes-angular|route]] comme :

```js
const routes: Routes = [
  { path: 'accueil', component: AccueilComponent },
  { path: 'details', component: DetailsComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

Lorsqu'on clique sur le lien « Accueil », `AccueilComponent` est affiché à l'endroit où se trouve le `router-outlet` dans le template ; de même pour « Détails » avec `DetailsComponent`.

## Points à noter

1. **[[routes-imbriquees-en-angular|Routes imbriquées]]** : pour des sous-routes, on peut utiliser plusieurs instances de `router-outlet`, chacune affichant le composant de la route correspondant à son niveau d'imbrication.
2. **[[routeroutlet-nommes|Outlets nommés]]** : pour des vues principales multiples (vue latérale et vue principale par exemple), on peut nommer chaque `router-outlet` et configurer les routes en conséquence.

L'utilisation de `router-outlet` est essentielle pour créer des applications à page unique (SPA) avec Angular, car elle permet de gérer dynamiquement l'affichage des vues en fonction de l'URL du navigateur.

## Voir aussi

- [[routing-angular]]
- [[routes-angular]]
- [[routeroutlet-nommes]]
- [[routes-imbriquees-en-angular]]
