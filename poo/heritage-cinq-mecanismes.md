---
tags: [poo, conception, heritage]
---

# Les 5 mécanismes cachés derrière « héritage »

L'héritage de classe classique (Java, C#, C++, Python) fournit **cinq choses simultanément**. La plupart du temps on n'en veut qu'une ou deux — d'où le conseil de préférer la composition.

| # | Mécanisme | Ce que ça apporte | Alternative |
|---|---|---|---|
| 1 | **Sous-typage** | `Derived` utilisable là où `Base` est attendu | interface, trait, typage structurel |
| 2 | **Héritage d'interface** | réutilisation de la *signature* | composition d'interfaces |
| 3 | **Héritage d'implémentation** | réutilisation du *code* | délégation, mixins, fonctions libres |
| 4 | **Partage d'état / layout** | les champs du parent dans l'objet enfant | un simple champ membre |
| 5 | **[[open-recursion]]** | le code du parent appelle une méthode **redéfinie** par l'enfant | ⚠️ rien de natif |

## Pourquoi cette décomposition est utile

- Elle montre que **sous-typage ≠ héritage**. Beaucoup de langages les séparent : Go (interfaces structurelles), Rust (traits), TypeScript (structural typing).
- Elle isole ce qui est réellement irremplaçable : **seul le point 5** demande une simulation manuelle.
- Elle explique pourquoi [[go-embedding]] « ressemble » à de l'héritage : il fournit 2, 3 et 4 — mais **pas 5**.

## Exemple : ce qu'on voulait vraiment

```java
// On écrit ça pour réutiliser du code...
class CachedRepo extends SqlRepo { ... }

// ...alors qu'on voulait juste ça (points 1 + 3 découplés)
class CachedRepo implements Repo {
    private final Repo inner;              // composition
    CachedRepo(Repo inner) { this.inner = inner; }
    public User find(int id) { /* cache */ return inner.find(id); }
}
```

Bénéfice du second : `CachedRepo` marche avec **n'importe quel** `Repo` (SQL, HTTP, mock), et ne casse pas quand `SqlRepo` change son interne.

## Voir aussi

- [[heritage-vs-composition]]
- [[composition-delegation]]
- [[open-recursion]]
