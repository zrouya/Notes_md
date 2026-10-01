---
tags: [angular, routing]
---

# Routes imbriquées en Angular

Les routes enfants (`children`) servent quand **plusieurs niveaux de la page dépendent de l'URL** : un layout parent reste affiché, seule sa zone interne change.

## Exemple

```ts
{
  path: 'compte',
  component: AccountLayoutComponent,   // contient son propre <router-outlet>
  children: [
    { path: '', redirectTo: 'profil', pathMatch: 'full' },
    { path: 'profil', component: ProfileComponent },     // /compte/profil
    { path: 'commandes', component: OrdersComponent },   // /compte/commandes
  ]
}
```

```html
<!-- account-layout.component.html -->
<nav>
  <a routerLink="profil">Profil</a>
  <a routerLink="commandes">Commandes</a>
</nav>
<router-outlet />   <!-- outlet enfant -->
```

## Détails

- Chaque niveau de route rend son composant dans le `<router-outlet>` de son **parent**.
- `routerLink` sans `/` initial est **relatif** à la route courante.
- Usage typique : menus latéraux, onglets, sections d'espace utilisateur.
- Pour simplement afficher plusieurs composants sur une page, **pas besoin de routes enfants** : on les compose dans le template (voir [[arbre-composants-vs-arbre-routes]]).

## Voir aussi

- [[routeroutlet-angular]]
- [[configuration-des-routes-en-angular]]
- [[arbre-composants-vs-arbre-routes]]
