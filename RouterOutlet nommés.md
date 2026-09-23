#How-to 

Dans les scénarios les plus courants, il y a  un [[RouterOutlet Angular|router-outlet]] principal au niveau racine pour afficher la principale navigation basée sur les routes. Cependant, dans des situations plus complexes où il y a besoin de plusieurs vues principales (comme une vue latérale et une vue principale), il est possible d'utiliser des outlets nommés.

Avec les outlets nommés, vous pouvez avoir plusieurs `router-outlet` dans une seule vue, chacun avec un nom unique. Vous configurez ensuite vos routes pour cibler un outlet spécifique.

Exemple :

```html
<!-- app.component.html -->
<router-outlet></router-outlet> <!-- outlet par défaut -->
<router-outlet name="sidebar"></router-outlet> <!-- outlet nommé 'sidebar' -->
```

[[Configuration des routes en Angular|Configuration des routes]] :

```js
const routes: Routes = [
  { path: 'accueil', component: AccueilComponent },
  { path: 'sidebar', component: SidebarComponent, outlet: 'sidebar' }
];
```

Avec cette configuration, lorsque vous naviguez vers `/accueil`, le `AccueilComponent` est affiché dans l'outlet par défaut. Si vous naviguez vers `/sidebar`, le `SidebarComponent` est affiché dans l'outlet nommé "sidebar".