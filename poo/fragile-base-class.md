---
tags: [poo, conception, heritage, anti-pattern]
---

# Modes de défaillance de l'héritage

Les griefs contre l'héritage d'implémentation ne sont pas esthétiques : ce sont des modes de défaillance documentés.

## 1. Fragile base class problem

Le parent ne peut plus changer son implémentation interne sans casser ses enfants, car ceux-ci dépendent de *comment* il est écrit (réutilisation **white-box**, cf. [[open-recursion]]).

```java
class Counter {
    int n;
    void add(String s)          { n++; }
    void addAll(List<String> l) { for (var s : l) add(s); }   // v1 : délègue à add()
}
class LoggingCounter extends Counter {
    @Override void add(String s)          { log(s); super.add(s); }
    @Override void addAll(List<String> l) { log(l); super.addAll(l); }
}
// v2 du parent : addAll fait n += l.size() sans appeler add()
// → LoggingCounter perd des logs. Aucune erreur de compilation.
// (Et en v1, les éléments étaient loggués DEUX fois.)
```

C'est l'exemple canonique de *Effective Java* (item 18) : `HashSet` + comptage d'insertions.

## 2. Encapsulation percée

`protected` est une **seconde API publique de fait** : non documentée, non testée, mais que tu ne peux plus changer.

## 3. Anti-pattern « call super »

L'enfant *doit* appeler `super.foo()`, souvent à un endroit précis — rien ne le vérifie. Contrat implicite non exprimable dans le type.

## 4. Explosion combinatoire

2 axes de variation orthogonaux × héritage simple = N×M classes.
`ScrollableWindow`, `BorderedWindow`, `ScrollableBorderedWindow`... C'est exactement le problème que résout le pattern **Bridge** — par composition.

## 5. Problème yo-yo

Pour comprendre un comportement, il faut lire 6 fichiers en remontant/descendant la hiérarchie. Le code d'un objet n'est plus localisé.

## 6. Hiérarchies « is-a » fausses

Voir [[principe-substitution-liskov]].

## À noter

[[go-embedding|L'embedding Go]] atténue mais n'élimine pas ces problèmes : ajouter une méthode au type embarqué change le method set du type englobant, peut créer une ambiguïté ou faire satisfaire silencieusement une interface non voulue.

## Voir aussi

- [[heritage-vs-composition]]
- [[composition-delegation]]
