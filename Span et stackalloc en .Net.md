---
tags: [dotnet, csharp, memoire, performance]
---

# Span et stackalloc en .Net

Mécanismes permettant de travailler sur des buffers **sans allouer sur le tas** — le levier concret de la distinction [[Stack et Heap en .Net|pile / tas]].

## stackalloc

Alloue un bloc sur la **pile** du thread courant :

```cs
Span<byte> buffer = stackalloc byte[256];  // zéro allocation GC
```

À réserver aux **petites tailles fixes**. Un `stackalloc` de taille variable non bornée est un chemin direct vers le `StackOverflowException` (pile ≈ 1 Mo).

```cs
// Garde-fou classique
Span<char> buf = n <= 128 ? stackalloc char[128] : new char[n];
```

## Span&lt;T&gt; et ReadOnlySpan&lt;T&gt;

Vue typée sur une zone mémoire contiguë, quelle qu'en soit l'origine : tableau, `stackalloc`, mémoire non managée, chaîne.

```cs
ReadOnlySpan<char> s = "2026-09-19".AsSpan();
int annee = int.Parse(s.Slice(0, 4));   // pas de sous-chaîne allouée
```

`Slice` ne copie rien : il déplace un pointeur et une longueur.

## ref struct : la garantie du compilateur

`Span<T>` est un **`ref struct`** — le seul type que le compilateur garantit ne jamais atterrir sur le tas. D'où ses restrictions :

- ne peut pas être champ d'une classe
- ne peut pas être capturé dans un lambda
- ne peut pas traverser un `await` ni un `yield` ([[Programmation Asynchrone en .Net]])
- ne peut pas être [[Boxing et Unboxing en CSharp|boxé]]

Pour les cas asynchrones, utiliser `Memory<T>` / `ReadOnlyMemory<T>`, qui sont des structs ordinaires.

## Voir aussi

- [[Stack et Heap en .Net]]
- [[Garbage Collector .Net]]
- [[Types Valeur et Types Référence en CSharp]]
