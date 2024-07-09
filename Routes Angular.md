
**L'objet Routes** est un tableau de configurations de routes. Chaque configuration est une **paire clé/valeur** qui associe un chemin (via la propriété `path`) à un composant (via la propriété `component`).
    
**[[Routes imbriquées en Angular|Arborescence]]** : L'objet `Routes` peut représenter une arborescence de routes grâce à la notion de routes enfants (`children`). Cela permet d'avoir des sous-routes ou des segments de routes imbriqués.
    
**Composant Route** : Pour chaque chemin, vous associez un composant Angular qui doit être affiché lorsque ce chemin est activé. Ce composant est chargé et inséré à l'endroit où se trouve la directive [[RouterOutlet Angular|router-outlet]] dans le template.
    
**Autres configurations** : En plus du couple basique `path`/`component`, il y a d'autres [[Configuration des routes en Angular|configurations possibles]] pour les routes, comme :
    
    - `redirectTo` : pour rediriger vers une autre route.
    - `pathMatch` : détermine comment le routeur doit matcher le chemin avec l'URL.
    - `canActivate` : pour la protection des routes à l'aide de guards.
    - `loadChildren` : pour le chargement à la volée (lazy loading) des modules.
    - Et bien d'autres.

**Route par défaut et routes jokers** : Vous pouvez également définir une route par défaut (généralement redirigée vers une page d'accueil ou similaire) et une route joker (wildcard) pour gérer les URL non reconnues.
    

Exemple :

```js
const routes: Routes = [
  { path: '', redirectTo: '/accueil', pathMatch: 'full' },
  { path: 'accueil', component: AccueilComponent },
  { path: '**', component: PageNotFoundComponent } // Route joker pour les URL non reconnues
];
```

Donc, pour résumer, vous avez parfaitement compris : le système de routage d'Angular permet d'associer des chemins d'URL à des composants spécifiques et d'afficher ces composants en fonction de l'URL active.
