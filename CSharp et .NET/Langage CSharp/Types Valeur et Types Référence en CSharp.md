---
tags: [csharp, dotnet, types, memoire]
---

# Types valeur et types référence en C#

Distinction fondamentale de [[CSharp|C#]] : elle détermine **comment une variable est copiée** et où vit la donnée.

## Comparaison

| | Type référence (`class`, `record`) | Type valeur (`struct`, `enum`) |
|---|---|---|
| La variable contient | une **adresse** | la **donnée elle-même** |
| Assignation `b = a` | copie de la référence → même objet | copie de tous les champs → objet distinct |
| Emplacement | toujours le [[Stack et Heap en .Net\|tas]] | là où vit son conteneur |
| `null` | possible | impossible (sauf `Nullable<T>` / `T?`) |
| Héritage | oui | non (interfaces seulement) |
| Égalité par défaut | référentielle | structurelle (par réflexion si non surchargée) |
| Constructeur sans paramètre | implicite | toujours présent, met tout à zéro |

## Conséquence sur la mutation

```cs
struct PointS { public int X, Y; }

var liste = new List<PointS> { new() { X = 1 } };
liste[0].X = 5;   // ne compile pas : liste[0] renvoie une COPIE
```

C'est la source classique de bugs silencieux avec les structs mutables — d'où la recommandation `readonly record struct`.

## Le mythe à démonter

> « Les types valeur vont sur la pile. »

**Faux.** La règle correcte : *un type valeur vit là où vit son conteneur*.

```cs
class Conteneur
{
    public int Compteur;    // sur le TAS, dans l'objet Conteneur
    public Point Position;  // struct, sur le TAS aussi
}
int[] tableau = new int[1000];  // les 1000 int sont sur le TAS
```

Une struct locale *peut* être sur la pile, dans un registre, ou supprimée par le JIT.

## Voir aussi

- [[Stack et Heap en .Net]]
- [[Boxing et Unboxing en CSharp]]
- [[Records en CSharp]]
- [[Record Struct en CSharp]]
