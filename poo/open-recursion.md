---
tags: [poo, conception, heritage, go]
---

# Open recursion (liaison tardive de `this`)

**La** subtilité qui distingue l'héritage de la composition : dans le corps d'une méthode du parent, `this` désigne l'objet **réel** (l'enfant). Un appel de méthode y est donc résolu dynamiquement vers la redéfinition de l'enfant.

## Le test décisif

```java
// Java — héritage
class Base {
    String name()  { return "base"; }
    String greet() { return "hello " + name(); }   // appel via this
}
class Derived extends Base {
    @Override String name() { return "derived"; }
}
new Derived().greet();   // → "hello derived"   ← open recursion
```

```go
// Go — embedding (composition)
type Base struct{}
func (Base) Name() string    { return "base" }
func (b Base) Greet() string { return "hello " + b.Name() }

type Derived struct{ Base }
func (Derived) Name() string { return "derived" }

Derived{}.Greet()   // → "hello base"   ← PAS d'open recursion
```

**Pourquoi ?** La méthode `Greet` promue est un wrapper qui appelle `d.Base.Greet()` : le receveur est de type `Base`, donc `b.Name()` résout **statiquement** vers `Base.Name`. `Derived.Name` ne *redéfinit* pas, il **masque** (shadowing).

## Simuler l'open recursion en composition

Il faut ré-injecter explicitement la référence au « soi » complet (*self-injection*) :

```go
type Namer interface{ Name() string }

type Base struct{ Self Namer }                          // ← câblage manuel
func (b Base) Greet() string { return "hello " + b.Self.Name() }
```

C'est ce câblage que l'héritage rendait invisible — pour le meilleur (concision) et pour le pire ([[fragile-base-class]]).

## À retenir

- Open recursion = le mécanisme qui rend le pattern [[template-method-vs-strategy|Template Method]] possible.
- C'est aussi la source du couplage : le parent **dépend** de l'enfant, donc modifier le parent casse les enfants.
- Un parent qui n'appelle jamais de méthode surchargeable ⇒ la délégation suffit toujours.

## Voir aussi

- [[heritage-cinq-mecanismes]]
- [[go-embedding]]
- [[template-method-vs-strategy]]
