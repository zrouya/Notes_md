---
tags: [angular, signals, state-management]
---

# Angular Signals

Un signal est une **valeur réactive** : quand on la lit, Angular enregistre qui l'a lue ; quand elle change, il met à jour **uniquement** ces consommateurs. Base de la [[state-managment-angular|détection de changements]] moderne (mode zoneless).

## Les trois primitives

```ts
import { signal, computed, effect } from '@angular/core';

const prix = signal(100);                  // valeur modifiable
const quantite = signal(3);

prix();                                    // lecture → 100
prix.set(120);                             // remplacement
quantite.update(q => q + 1);               // à partir de l'ancienne valeur

const total = computed(() => prix() * quantite());   // dérivée, lecture seule

effect(() => localStorage.setItem('total', String(total())));  // effet de bord
```

## `computed`

- Dépendances **détectées automatiquement** à l'exécution
- **Paresseux** : calculé seulement quand on le lit
- **Mémorisé** : recalculé seulement si une dépendance change
- **Sans glitch** : jamais d'état intermédiaire incohérent

## Dans un composant

```ts
export class PanierComponent {
  articles = signal<Article[]>([]);
  total = computed(() => this.articles().reduce((s, a) => s + a.prix, 0));

  retirer(id: number) {
    this.articles.update(list => list.filter(a => a.id !== id));
  }
}
```

```html
<p>Total : {{ total() }} €</p>
@if (total() > 100) { <p>Livraison offerte</p> }
```

## Règle d'usage

- `signal` pour l'**état**
- `computed` pour tout ce qui en **découle**
- `effect` uniquement pour **sortir** du monde Angular (storage, logs, lib tierce)

## Voir aussi

- [[angular-signals-avance]] — linkedSignal, resource, untracked
- [[angular-signals-pieges]]
- [[signals-vs-rxjs]]
- [[angular-component-inputs-outputs]] — `input()`, `output()`, `model()`
