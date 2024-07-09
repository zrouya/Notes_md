#How-to 

La directive `router-outlet` est un élément fondamental du système de [[Routing Angular|routage d'Angular]]. Elle sert de marqueur ou de point d'ancrage dans les templates où le routeur doit afficher le composant correspondant à la route activée.

Quand vous naviguez d'une route à une autre, le routeur d'Angular détecte le composant qui doit être affiché pour cette route, et le place dans le `router-outlet`.

Un exemple simple d'utilisation de `router-outlet` pourrait être :

```html
<!-- app.component.html -->
<nav>
  <a routerLink="/accueil">Accueil</a>
  <a routerLink="/details">Détails</a>
</nav>

<router-outlet></router-outlet>
```

Avec une configuration de [[Routes Angular|route]] comme :

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

Lorsque vous cliquez sur le lien "Accueil", le `AccueilComponent` est affiché à l'endroit où se trouve le `router-outlet` dans le template. De même, si vous cliquez sur "Détails", le `DetailsComponent` est affiché à la place.

**Points à noter**:

1. **[[Routes imbriquées en Angular|Routes imbriquées]]** : Si vous avez des configurations de routes imbriquées (c'est-à-dire des sous-routes), vous pouvez utiliser plusieurs instances de `router-outlet`. Dans ce cas, chaque instance affichera le composant de la route correspondant à son niveau d'imbrication.
    
2. **[[RouterOutlet nommés|Nomination]]** : Dans des situations plus complexes où vous avez besoin de multiples vues principales (par exemple, une vue latérale et une vue principale), vous pouvez donner un nom à chaque `router-outlet` et configurer vos routes en conséquence.
    

L'utilisation de la directive `router-outlet` est essentielle pour créer des applications à page unique (SPA) avec Angular, car elle permet de gérer dynamiquement l'affichage des vues en fonction de l'URL du navigateur.