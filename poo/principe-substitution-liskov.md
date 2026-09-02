---
tags: [poo, conception, solid, heritage]
---

# Principe de substitution de Liskov (LSP)

Le « L » de SOLID. Si `S` est un sous-type de `T`, alors tout programme correct utilisant `T` doit rester correct en lui passant un `S` — **sans le savoir**.

## Ce que ça contraint vraiment

Un sous-type peut :

| | Préconditions | Postconditions | Invariants | Exceptions |
|---|---|---|---|---|
| Sous-type autorisé à | **affaiblir** | **renforcer** | préserver | ne pas en ajouter de nouvelles |

Traduction : *exiger moins, garantir plus*. L'inverse casse l'appelant.

## Contre-exemples célèbres (bibliothèque Java standard)

```java
Properties p = new Properties();       // extends Hashtable<Object,Object>
p.put(42, new Date());                 // ✅ compile — mais p.getProperty() casse
                                       //    l'invariant « clés et valeurs = String »

Stack<Integer> s = new Stack<>();      // extends Vector<Integer>
s.add(0, 99);                          // ✅ compile — insertion au milieu d'une PILE
```

Dans les deux cas l'héritage a été utilisé pour **réutiliser du code**, et a exposé par accident une API qui viole l'invariant du sous-type.

## Le classique Carré / Rectangle

```java
class Rectangle { void setWidth(int w); void setHeight(int h); }
class Square extends Rectangle { /* w et h doivent rester égaux */ }

void f(Rectangle r) { r.setWidth(5); r.setHeight(4); assert r.area() == 20; }
f(new Square());   // ❌ échoue
```

Un carré **est** un rectangle en géométrie, pas dans un modèle **mutable**. Moralité : « is-a » est une relation sur les *contrats de comportement*, pas sur le vocabulaire du domaine.

## Application pratique

- Si tu dois écrire dans la doc du sous-type « ne pas appeler telle méthode héritée » → LSP violé, utiliser la [[composition-delegation|composition]].
- Un `throw new UnsupportedOperationException()` dans une surcharge est un signal de violation.

## Voir aussi

- [[heritage-vs-composition]]
- [[fragile-base-class]]
