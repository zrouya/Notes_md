---
tags: [index, poo, conception]
---

# Index — POO & Conception

Notes sur l'héritage, la composition et les mécanismes de réutilisation de code.

## Parcours de lecture conseillé

1. [[heritage-vs-composition]] → 2. [[heritage-cinq-mecanismes]] → 3. [[open-recursion]] → 4. [[go-embedding]]

## Notes

| Note | Description |
|------|-------------|
| [[heritage-vs-composition]] | Note pivot : le vrai débat, la formule, heuristiques de décision |
| [[heritage-cinq-mecanismes]] | Les 5 choses que l'héritage fait en même temps, et leurs alternatives |
| [[open-recursion]] | La subtilité décisive : liaison tardive de `this`. Test Java vs Go |
| [[fragile-base-class]] | Modes de défaillance : base fragile, `call super`, explosion N×M, yo-yo |
| [[principe-substitution-liskov]] | LSP : `Stack extends Vector`, carré/rectangle, règles de variance |
| [[composition-delegation]] | Le trio composition + délégation + interface, et son boilerplate |
| [[template-method-vs-strategy]] | Le cas légitime de l'héritage et sa traduction en composition |
| [[go-embedding]] | Go n'a pas d'héritage : embedding, promotion, shadowing ≠ override |
| [[heritage-langages-modernes]] | Rust, Kotlin, Scala, Swift, C++ CRTP : comment ils tranchent |

## Voir aussi

- [[design-patterns]]
- [[methodes-d-extensions-en-csharp]] — étendre un type sans en hériter
