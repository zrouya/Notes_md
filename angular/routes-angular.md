---
tags: [angular, routing, routes]
---

# Routes Angular

L'objet `Routes` est un tableau de configurations de routes. Chaque configuration est une paire clé/valeur qui associe un chemin (`path`) à un composant (`component`).

- **[[routes-imbriquees-en-angular|Arborescence]]** : l'objet `Routes` peut représenter une arborescence grâce aux routes enfants (`children`), permettant des sous-routes ou segments de routes imbriqués.
- **Composant Route** : pour chaque chemin, un composant Angular est chargé et inséré à l'endroit où se trouve la directive [[routeroutlet-angular|router-outlet]] dans le template.
- **Autres configurations** : en plus du couple `path`/`component`, on trouve notamment `redirectTo` (redirection), `pathMatch` (stratégie de correspondance), `canActivate` (protection via guards), `loadChildren` (lazy loading de modules), et bien d'autres — voir [[configuration-des-routes-en-angular|Configuration des routes]].
- **Route par défaut et route joker** : une route par défaut (redirigée par exemple vers l'accueil) et une route joker (`**`) permettent de gérer les URL non reconnues.

## Exemple

```js
const routes: Routes = [
  { path: '', redirectTo: '/accueil', pathMatch: 'full' },
  { path: 'accueil', component: AccueilComponent },
  { path: '**', component: PageNotFoundComponent } // Route joker pour les URL non reconnues
];
```

Le système de routage d'Angular permet ainsi d'associer des chemins d'URL à des composants spécifiques et d'afficher ces composants en fonction de l'URL active.

## Voir aussi

- [[routeroutlet-angular]]
- [[routes-imbriquees-en-angular]]
- [[configuration-des-routes-en-angular]]
