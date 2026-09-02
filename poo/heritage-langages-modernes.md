---
tags: [poo, conception, heritage, composition, langages]
---

# L'héritage dans les langages récents

Tendance nette : **garder le sous-typage, remplacer l'héritage d'implémentation** par des mécanismes plus fins. Personne ne conclut « composition pure et rien d'autre » ; tout le monde conclut « réutilisation de code découplée de la hiérarchie de types ».

| Langage | Héritage d'implémentation | Mécanisme de réutilisation |
|---|---|---|
| **Go** | ❌ | embedding + promotion de méthodes ([[go-embedding]]) |
| **Rust** | ❌ | traits avec méthodes par défaut, `#[derive]` |
| **Kotlin** | ✅ (mais classes `final` par défaut) | délégation `by` dans le langage |
| **Scala** | ✅ | traits + linéarisation (mixins empilables) |
| **Swift** | ✅ (classes) | protocol extensions |
| **TypeScript** | ✅ | typage structurel + mixins par fonctions |
| **C++** | ✅ (multiple) | CRTP = polymorphisme **statique**, sans vtable |

## Exemples parlants

```rust
// Rust : le trait porte l'implémentation par défaut, pas de hiérarchie
trait Greet {
    fn name(&self) -> String;
    fn greet(&self) -> String { format!("hello {}", self.name()) }  // défaut
}
// self est le type concret → on RETROUVE l'open recursion, sans héritage
```

```kotlin
// Kotlin : `by` supprime le boilerplate de délégation
class CachedRepo(private val inner: Repo) : Repo by inner {
    override fun find(id: Int) = cache[id] ?: inner.find(id)
}
```

```cpp
// C++ CRTP : template method sans virtual, résolu à la compilation
template <class D> struct Base {
    std::string greet() { return "hello " + static_cast<D*>(this)->name(); }
};
struct Derived : Base<Derived> { std::string name() { return "derived"; } };
```

## Nuances pour le débat

- Les **traits Rust** montrent qu'on peut avoir de l'[[open-recursion]] **sans** héritage : le défaut appelle `self.name()`, résolu sur le type concret. C'est la meilleure réponse au dilemme.
- Kotlin rend les classes `final` par défaut (`open` requis) : l'héritage devient un **choix explicite du concepteur**, pas un défaut.
- L'absence d'héritage n'est pas toujours confortable : l'implémentation du DOM dans Servo (Rust) est l'exemple classique où le manque d'héritage d'implémentation a été vécu comme une gêne.

## Conclusion

Go/Rust ne prouvent pas que l'héritage est *inutile* ; ils prouvent qu'un langage majeur peut réussir **sans héritage d'implémentation**. Nuance importante dans une discussion.

## Voir aussi

- [[heritage-vs-composition]]
- [[go-embedding]]
- [[composition-delegation]]
