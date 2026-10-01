---
tags: [angular, routing]
---

# Paramètres de route et navigation programmatique

Lire les paramètres de l'URL (`/produits/:id`, `?tri=prix`) et naviguer depuis le code.

## Lire un paramètre

```ts
// Avec provideRouter(routes, withComponentInputBinding())
export class ProductDetailComponent {
  id = input<string>();          // reçoit :id (ou un query param du même nom)
}

// Méthode classique
export class ProductDetailComponent {
  private route = inject(ActivatedRoute);
  id$ = this.route.paramMap.pipe(map(p => p.get('id')));
  tri$ = this.route.queryParamMap.pipe(map(p => p.get('tri')));
}
```

## Naviguer depuis le code

```ts
private router = inject(Router);

this.router.navigate(['/produits', 42], { queryParams: { tri: 'prix' } });
// → /produits/42?tri=prix

// Modifier un query param sans changer de page
this.router.navigate([], { queryParams: { chat: 7 }, queryParamsHandling: 'merge' });
```

## Piège : réutilisation du composant

De `/produits/1` à `/produits/2`, Angular **réutilise la même instance** du composant. Lire le paramètre une seule fois dans `ngOnInit` (`snapshot`) ne suffit pas : il faut réagir aux changements (`paramMap` observable ou `input()` signal).

## Voir aussi

- [[routing-angular]]
- [[etat-partage-service-vs-url]]
- [[angular-signals]]
