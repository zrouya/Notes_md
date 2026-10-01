---
tags: [angular, routing, router-outlet]
---

# RouterOutlet nommés

Dans les scénarios les plus courants, il y a un [[routeroutlet-angular|router-outlet]] principal au niveau racine pour afficher la navigation principale basée sur les routes. Pour des situations plus complexes nécessitant plusieurs vues principales (vue latérale et vue principale par exemple), on utilise des outlets nommés : plusieurs `router-outlet` dans une seule vue, chacun avec un nom unique, ciblés individuellement par la configuration des routes.

## Exemple

```html
<!-- app.component.html -->
<router-outlet />                   <!-- outlet par défaut (primary) -->
<router-outlet name="panneau" />    <!-- outlet nommé -->
```

[[configuration-des-routes-en-angular|Configuration des routes]] :

```ts
const routes: Routes = [
  { path: 'produits/:id', component: ProductDetailComponent },
  { path: 'chat/:id', component: ChatComponent, outlet: 'panneau' }
];
```

## Syntaxe d'URL et navigation

```
/produits/42(panneau:chat/7)
```

```ts
this.router.navigate([{ outlets: { panneau: ['chat', 7] } }]);   // ouvrir
this.router.navigate([{ outlets: { panneau: null } }]);          // fermer
```

## Quand l'utiliser ?

- Plusieurs zones **indépendantes** qui doivent chacune être pilotées par l'URL.
- Rarement nécessaire : les URL deviennent peu lisibles.
- Alternatives plus simples : état dans un **service** ou un **query param** (`?chat=7`), voir [[etat-partage-service-vs-url]].

## Voir aussi

- [[routeroutlet-angular]]
- [[configuration-des-routes-en-angular]]
- [[arbre-composants-vs-arbre-routes]]
