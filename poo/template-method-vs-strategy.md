---
tags: [poo, conception, design-pattern, heritage]
---

# Template Method vs Strategy

Le cas où l'héritage est réellement l'outil adapté — et sa traduction exacte en composition. Le passage de l'un à l'autre est le meilleur exercice pour comprendre [[open-recursion]].

## Template Method (héritage)

Le parent **pilote l'algorithme** et laisse des trous que l'enfant remplit.

```java
abstract class Report {
    final String render() {                 // final = squelette non modifiable
        return header() + body() + footer();
    }
    String header() { return "=== rapport ==="; }   // hook avec défaut
    abstract String body();                          // trou obligatoire
    String footer() { return ""; }                   // hook optionnel
}

class SalesReport extends Report {
    @Override String body() { return "CA : 42k"; }
}
```

✅ Concis, découvrable (l'IDE liste les `abstract` à implémenter), défauts gratuits.
❌ Un seul axe de variation, `Report` ne peut plus changer `render()` sans risque, `SalesReport` ne peut hériter d'autre chose.

## Strategy / hooks injectés (composition)

```java
record ReportSpec(Supplier<String> header, Supplier<String> body, Supplier<String> footer) {}

String render(ReportSpec s) {                 // fonction libre, pas de hiérarchie
    return s.header().get() + s.body().get() + s.footer().get();
}

render(new ReportSpec(DEFAULT_HEADER, () -> "CA : 42k", EMPTY_FOOTER));
```

✅ Axes de variation combinables librement, testable morceau par morceau, aucun couplage d'héritage.
❌ Les défauts doivent être passés explicitement, et rien ne force à fournir chaque hook (moins découvrable).

## Comment choisir

| Choisir Template Method si | Choisir Strategy si |
|---|---|
| Beaucoup de comportement partagé, peu de variation | Peu de partagé, beaucoup de variation |
| Ensemble de variantes **fermé** et co-évoluant | Variantes ajoutées par des tiers |
| Parent et enfants dans la même unité de release | Le parent est une lib publique |
| 1 seul axe de variation | Plusieurs axes orthogonaux |

## Voir aussi

- [[open-recursion]]
- [[composition-delegation]]
- [[Design Patterns]]
