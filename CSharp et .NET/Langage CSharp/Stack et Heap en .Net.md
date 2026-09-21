---
tags: [dotnet, csharp, memoire, performance]
---

# Stack et Heap en .Net

Les deux zones mémoire d'un processus [[DotNet|.Net]]. La différence tient à la **durée de vie** de la donnée, pas à son type.

## La pile (stack)

- **Une pile par [[Threads en .Net|thread]]**, bloc contigu réservé à sa création (1 Mo par défaut sous Windows).
- Fonctionnement **LIFO** : chaque appel empile une *stack frame* (arguments, locales, adresse de retour).
- Désallouer = déplacer un pointeur. Pas de fragmentation, pas de recherche → très rapide.

```
   ┌──────────────────┐  adresses hautes
   │ frame de A       │  x = 1
   ├──────────────────┤
   │ frame de B       │  y = 1, z = 2
   └──────────────────┘  ← stack pointer (croît vers le bas)
```

- Durée de vie liée à la **portée lexicale** ; taille connue à la compilation.
- Débordement → `StackOverflowException` **non rattrapable** : le processus meurt (pas de `catch` depuis .Net 2.0).

## Le tas (heap)

- Partagé par tous les threads du processus. Accueille toutes les instances de types référence.
- L'allocation est un simple incrément de l'*allocation pointer* : le tas .Net est **compacté**, contrairement à `malloc` en C.
- Le coût réel n'est pas dans l'allocation mais dans le [[Garbage Collector .Net|ramassage]] qui suivra.
- Chaque objet porte un en-tête (pointeur de table de méthodes + sync block) : **16 octets** de surcoût sur x64.

## Comment les deux s'articulent

```cs
void M()
{
    int n = 42;                                    // pile
    var p = new Personne { Nom = "Zakir" };        // p = adresse (pile)
}                                                  // objet Personne → tas
```

## Cas où un type valeur atterrit sur le tas

- Champ d'une classe, élément d'un tableau
- [[Boxing et Unboxing en CSharp|Boxing]] vers `object` ou une interface
- **Closure** : une variable capturée par un lambda est hissée dans une classe générée
- **`async` / `yield`** : la frame disparaît à chaque `await`, les locales vivent dans la machine à états ([[Programmation Asynchrone en .Net]])

## À retenir

La bonne question n'est pas « pile ou tas » mais **« combien de temps cet objet survit-il »**. On ne choisit pas la pile : on choisit un type, et le JIT décide.

## Voir aussi

- [[Types Valeur et Types Référence en CSharp]]
- [[Garbage Collector .Net]]
- [[Span et stackalloc en .Net]]
