---
tags: [angular, signals, rxjs]
---

# Signaux vs RxJS

> Un **signal** représente une _valeur_ qui change. Un **Observable** représente une _suite d'événements_ dans le temps.

```
Signal :      ──[ 3 ]────────[ 5 ]──────[ 8 ]──►   « combien maintenant ? » → 8
Observable :  ───●───●──●──────────●───●──|──►     « que s'est-il passé, et quand ? »
```

## Comparatif

| | Signal | Observable |
|---|---|---|
| Nature | Valeur courante | Flux d'émissions |
| Valeur actuelle | Toujours : `s()` | Pas forcément (sauf `BehaviorSubject`) |
| Lecture | Synchrone | `subscribe`, asynchrone |
| Notion de temps | Aucune | Spécialité (`debounceTime`, `delay`…) |
| Désabonnement | Rien à gérer | Nécessaire (fuites mémoire) |
| Fin / erreur | N'existent pas | `complete` / `error` |
| Dépendances | Automatiques | Explicites (`combineLatest`…) |
| Modèle | Push-pull (paresseux) | Push |
| Diamant de dépendances | Cohérent (pas de glitch) | Émissions intermédiaires incohérentes |

## Glitch en RxJS

```ts
b$ = a$.pipe(map(x => x * 2));
c$ = a$.pipe(map(x => x + 10));
d$ = combineLatest([b$, c$]);
a$.next(2);   // d$ émet [4, 11] (incohérent) puis [4, 12]
```

Avec `computed`, `d` n'est recalculé qu'une fois → `[4, 12]`.

## Choisir

| Question | Outil |
|---|---|
| Quelle est la valeur maintenant ? | **signal** |
| Qu'est-ce qui en découle ? | **computed** |
| Que faire *quand* un événement arrive, dans quel ordre ? | **RxJS** |
| Afficher le résultat d'un flux | **toSignal** |

RxJS pour : debounce, annulation (`switchMap`), file (`concatMap`), retry, polling, WebSockets.

## Voir aussi

- [[rxjs-signals-interop]]
- [[angular-signals]]
