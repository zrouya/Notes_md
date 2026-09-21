---
tags: [csharp, dotnet, types, performance]
---

# Record struct en C#

Variante **type valeur** du [[Records en CSharp|record]], introduite en [[CSharp|C#]] 10. Sucre syntaxique au-dessus de `struct` : même nature (pile/inline, pas d'héritage, jamais `null`), mais avec les membres générés.

## Les trois variantes

| Déclaration | Nature | Depuis |
|---|---|---|
| `record` / `record class` | type référence | C# 9 |
| `record struct` | type valeur, propriétés **mutables** | C# 10 |
| `readonly record struct` | type valeur immuable | C# 10 |

## Ce que `record struct` ajoute à une `struct` nue

```cs
record struct Point(int X, int Y);

var a = new Point(1, 2);
var b = a with { Y = 3 };   // impossible sur une struct nue
a == b;                     // false ; ne compilerait pas sur une struct nue
```

- `Equals(T)` / `IEquatable<T>` typé → **sans réflexion ni boxing**
- `==` et `!=` (une `struct` nue n'a aucun opérateur d'égalité)
- `GetHashCode`, `ToString`, `Deconstruct`, `with`

## Le piège de l'immuabilité

`record struct Point(int X, int Y)` génère des propriétés **`get; set;`** — l'inverse du `record class` qui génère `init`. Pour l'immuabilité, il faut écrire explicitement :

```cs
readonly record struct Point(int X, int Y);
```

## Pourquoi éviter `ValueType.Equals`

Une `struct` non surchargée hérite de `ValueType.Equals`, qui passe par la **réflexion** dès qu'elle contient un champ référence : lent et allouant. Le `record struct` génère la comparaison champ par champ à la compilation.

## Quand l'utiliser

Petite valeur (≈ ≤ 16 octets, 2-4 champs) sur un chemin chaud : coordonnées, identifiants fortement typés, montants. Au-delà, le coût de **copie** à chaque passage de paramètre dépasse le gain d'allocation évitée → revenir au `record class`.

## Voir aussi

- [[Records en CSharp]]
- [[Types Valeur et Types Référence en CSharp]]
- [[Stack et Heap en .Net]]
