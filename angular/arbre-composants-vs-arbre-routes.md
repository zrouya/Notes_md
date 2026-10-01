---
tags: [angular, composants, routing]
---

# Arbre de composants vs arbre de routes

Une application Angular est **toujours** un arbre de composants avec **une racine** (le composant passé à `bootstrapApplication`). Mais afficher plusieurs composants sur une page ne passe **pas** forcément par des routes enfants.

## Deux arbres distincts

| | Arbre de composants | Arbre de routes |
|---|---|---|
| Défini par | Les **templates** (`<app-header />`) | La config (`children`) |
| Dépend de l'URL | Non : toujours affiché | Oui |
| Usage | Composition classique de l'UI | Niveaux de page pilotés par l'URL |

## Exemple

```html
<!-- app.component.html -->
<app-header />
<app-sidebar />
<main>
  <router-outlet />     <!-- seule cette zone dépend de l'URL -->
</main>
<app-footer />
```

```
AppComponent (racine)
├── HeaderComponent          ← template (fixe)
├── SidebarComponent         ← template (fixe)
├── <router-outlet>          ← routé
│     └── ProductDetailComponent   (/produits/:id)
│           ├── ProductGalleryComponent   ← template
│           └── ReviewsComponent          ← template
└── FooterComponent          ← template (fixe)
```

## À retenir

- Le Router ne décide que **du contenu des `<router-outlet>`**.
- Tout le reste est de la composition de composants (inputs/outputs, services).
- [[routes-imbriquees-en-angular|Routes enfants]] : seulement si plusieurs niveaux dépendent de l'URL.
- [[routeroutlet-nommes|Outlets nommés]] : si plusieurs zones **indépendantes** dépendent de l'URL (rare).

## Voir aussi

- [[composants-angular]]
- [[routeroutlet-angular]]
- [[angular-component-inputs-outputs]]
