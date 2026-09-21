---
tags: [dotnet, memoire, performance, gc]
---

# Garbage Collector .Net

Le ramasse-miettes libère automatiquement les objets du [[Stack et Heap en .Net|tas]] devenus inatteignables.

## Principe : le parcours des racines

Le GC part des **racines** puis suit les références de proche en proche. Ce qui n'est pas atteint est mort.

Racines :
- variables des piles de tous les [[Threads en .Net|threads]] et registres
- champs statiques
- handles (GCHandle, interop)

## Les générations

Fondées sur l'observation que *la plupart des objets meurent jeunes*.

| Zone | Contenu | Collecte |
|---|---|---|
| **Gen 0** | objets neufs | très fréquente, ~microsecondes |
| **Gen 1** | survivants d'une collecte | intermédiaire, sert de tampon |
| **Gen 2** | survivants durables | rare et coûteuse (parcourt tout) |
| **LOH** | objets ≥ 85 000 octets | avec gen 2, **non compacté** par défaut |

Une collecte gen 0 ne balaye que la zone gen 0, promeut les survivants en gen 1 et compacte. C'est pourquoi allouer beaucoup de petits objets éphémères est acceptable en [[DotNet|.Net]].

## Les vrais coûts

- **Objet promu par accident en gen 2** : mis en cache, retenu par un event non désabonné, capturé par une closure longue durée. Bien plus cher qu'une allocation éphémère.
- **Fragmentation du LOH** : non compacté, donc il se fragmente réellement. Un service allouant en boucle des `byte[]` de 100 Ko gaspille de la mémoire → utiliser `ArrayPool<T>`.
- **Allocations invisibles** : le [[Boxing et Unboxing en CSharp|boxing]] est la source la plus fréquente.

## Voir aussi

- [[Stack et Heap en .Net]]
- [[Boxing et Unboxing en CSharp]]
- [[Span et stackalloc en .Net]]
