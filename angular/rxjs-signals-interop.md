---
tags: [angular, signals, rxjs]
---

# Interop RxJS ↔ signaux

Ponts fournis par `@angular/core/rxjs-interop` pour combiner les deux modèles.

## API

```ts
import { toSignal, toObservable, takeUntilDestroyed } from '@angular/core/rxjs-interop';

const params = toSignal(this.route.paramMap);             // Observable → signal
const recherche$ = toObservable(this.recherche);          // signal → Observable
source$.pipe(takeUntilDestroyed()).subscribe(...);        // désabonnement auto
```

- `toSignal` s'abonne et se désabonne automatiquement (lié au contexte d'injection).
- `initialValue` évite un `undefined` initial ; `requireSync: true` si l'Observable émet de façon synchrone (`BehaviorSubject`).

## Pattern : orchestrer en RxJS, exposer en signal

```ts
export class RechercheComponent {
  private api = inject(ApiService);

  recherche = signal('');                               // état

  resultats = toSignal(
    toObservable(this.recherche).pipe(                  // logique temporelle
      debounceTime(300),
      distinctUntilChanged(),
      switchMap(q => this.api.chercher(q)),
    ),
    { initialValue: [] }
  );

  nbResultats = computed(() => this.resultats().length);   // dérivé
}
```

```html
<input [value]="recherche()" (input)="recherche.set($any($event.target).value)" />
<p>{{ nbResultats() }} résultat(s)</p>
```

Sans besoin temporel (simple requête dépendant d'un signal) → `httpResource` suffit ([[angular-signals-avance]]).

## Voir aussi

- [[signals-vs-rxjs]]
- [[angular-signals]]
