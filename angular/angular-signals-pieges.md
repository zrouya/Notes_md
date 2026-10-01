---
tags: [angular, signals, pieges]
---

# Angular Signals : pièges classiques

## 1. Muter au lieu de remplacer

Un signal compare par référence : même objet = pas de changement.

```ts
this.articles().push(nouvel);                          // ❌ rien ne se met à jour
this.articles.update(list => [...list, nouvel]);       // ✅ nouveau tableau
this.user.update(u => ({ ...u, nom: 'Alice' }));       // ✅ nouvel objet
```

## 2. `effect` pour synchroniser des signaux

```ts
effect(() => this.total.set(this.prix() * this.quantite()));   // ❌
total = computed(() => this.prix() * this.quantite());          // ✅
```

Besoin d'un dérivé modifiable → `linkedSignal` ([[angular-signals-avance]]).

## 3. Oublier les parenthèses

```html
{{ total }}     <!-- ❌ affiche la fonction -->
{{ total() }}   <!-- ✅ -->
```

## 4. Dépendances involontaires

Dans `computed` / `effect`, **tout signal lu devient une dépendance**, y compris dans les fonctions appelées. Utiliser `untracked()` pour une simple lecture.

## 5. Lecture conditionnelle

```ts
computed(() => this.actif() ? this.a() : this.b());
```

Seules les dépendances **lues lors de la dernière exécution** sont suivies : c'est voulu, mais parfois surprenant.

## Voir aussi

- [[angular-signals]]
- [[angular-signals-avance]]
