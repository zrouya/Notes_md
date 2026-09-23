---
tags: [csharp, dotnet, memoire, performance]
---

# Boxing et Unboxing en C#

Le **boxing** convertit un type valeur en `object` ou en interface : il alloue une « boîte » sur le [[Stack et Heap en .Net|tas]] et y recopie la valeur. L'**unboxing** fait le chemin inverse.

## Exemple

```cs
int x = 42;
object o = x;      // boxing   → allocation sur le tas
int y = (int)o;    // unboxing → recopie
```

## Pourquoi ça coûte cher

- Une **allocation** par boxing → pression sur le [[Garbage Collector .Net|GC]]
- Une **copie** dans les deux sens
- Une **indirection** à chaque accès
- Le tout est **invisible dans le code source** : c'est l'ennemi silencieux des chemins chauds

## Sources courantes de boxing

- `List<object>`, `ArrayList`, `Hashtable` (collections non génériques)
- `string.Format("{0}", monInt)` et la concaténation avec des types valeur
- Une `struct` passée comme `IComparable`, `IEquatable` non typé, etc.
- `foreach` sur une collection non générique
- `ValueType.Equals` par réflexion sur une `struct` non surchargée

## Comment l'éviter

- Génériques partout : `List<int>` plutôt que `List<object>`
- Interpolation de chaînes moderne (`$"{x}"` sur .Net 6+ utilise `DefaultInterpolatedStringHandler`, sans boxing)
- Implémenter `IEquatable<T>` sur ses structs — ou utiliser [[Record Struct en CSharp|`readonly record struct`]] qui le fait pour vous
- Contraintes de type générique (`where T : struct`) pour éviter la conversion en interface

## Voir aussi

- [[Types Valeur et Types Référence en CSharp]]
- [[Stack et Heap en .Net]]
- [[Garbage Collector .Net]]
