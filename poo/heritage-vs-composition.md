---
tags: [poo, conception, heritage, composition]
---

# Héritage vs Composition

Note pivot du sujet. Le débat « héritage ou composition » est mal posé : l'héritage de classe fait **plusieurs choses à la fois** (voir [[heritage-cinq-mecanismes]]), et la vraie règle est de **découpler le sous-typage de la réutilisation de code**.

## La formule à retenir

> Interfaces pour le **polymorphisme**, composition pour la **réutilisation**.

- « Prefer composition over inheritance » : GoF (1994), puis Bloch, *Effective Java*, item 18.
- L'héritage n'est pas condamné pour son sous-typage, mais pour sa **réutilisation white-box** : l'enfant dépend de *comment* le parent est écrit, pas seulement de son contrat.

## Tout l'héritage est-il remplaçable par de la composition ?

Presque. Correspondances directes :

| Besoin | Sans héritage |
|---|---|
| Substituabilité | interface / trait / typage structurel |
| Réutiliser du code | délégation ([[composition-delegation]]) |
| Partager de l'état | un champ membre |
| Hiérarchie de contrats | composition d'interfaces |

**La seule chose qui ne se reproduit pas gratuitement : l'[[open-recursion]]** (le code du parent appelle une méthode redéfinie par l'enfant). C'est faisable par injection du `self`, mais c'est du câblage manuel — et le coût devient réel quand 40 sous-types partagent 90 % du comportement (toolkits GUI, DOM, nœuds d'AST).

## Heuristiques de décision

Le test « is-a / has-a » est trop faible. Poser plutôt :

1. Je veux la **substituabilité** ou juste du **code** ? Si juste du code → composition.
2. Le parent appelle-t-il du code de l'enfant ? Si **non**, la délégation suffit *toujours*.
3. Parent et enfants évoluent-ils ensemble, dans la même unité de release ? Si non → [[fragile-base-class]].
4. L'ensemble des variantes est-il **fermé** ? Si oui, penser aussi type somme / `sealed`.
5. Le sous-type respecte-t-il vraiment le contrat du parent ? → [[principe-substitution-liskov]]

## Voir aussi

- [[heritage-cinq-mecanismes]] — décomposition du concept
- [[open-recursion]] — la subtilité décisive
- [[fragile-base-class]] — modes de défaillance
- [[template-method-vs-strategy]] — le cas où l'héritage se justifie, et sa traduction
- [[go-embedding]] — « Go n'a pas d'héritage » : vrai, mais nuancé
- [[heritage-langages-modernes]] — comment Rust, Kotlin, Scala, Swift tranchent
