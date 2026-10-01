---
tags: [angular, signals]
---

# Angular Signals : API avancée

Primitives complémentaires à `signal` / `computed` / `effect` (voir [[angular-signals]]).

## `linkedSignal` : dérivé mais modifiable

Se comporte comme un `computed`, mais on peut l'écraser. Il se **réinitialise** quand sa source change.

```ts
const options = signal(['Petit', 'Moyen', 'Grand']);
const choix = linkedSignal(() => options()[0]);   // 'Petit'

choix.set('Grand');               // choix utilisateur
options.set(['S', 'M', 'L']);     // source modifiée → choix redevient 'S'
```

## `resource` / `httpResource` : données asynchrones

```ts
produitId = input.required<number>();
produit = httpResource<Produit>(() => `/api/produits/${this.produitId()}`);
```

```html
@if (produit.isLoading()) { <p>Chargement…</p> }
@if (produit.error()) { <p>Erreur</p> }
@if (produit.hasValue()) { <h1>{{ produit.value().nom }}</h1> }
```

Quand `produitId` change, la requête est **relancée** et la précédente est **annulée**.

## `untracked` : lire sans dépendre

```ts
effect(() => {
  const q = this.recherche();                  // dépendance suivie
  const user = untracked(() => this.user());   // simple lecture
  log(q, user);
});
```

## Autres

- `asReadonly()` : exposer un signal en lecture seule (encapsulation dans un service)
- `signal(v, { equal: (a, b) => ... })` : comparaison personnalisée (défaut : `Object.is`)
- `viewChild()`, `contentChildren()` : requêtes de template en signaux

## Voir aussi

- [[angular-signals]]
- [[rxjs-signals-interop]]
