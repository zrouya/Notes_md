---
tags: [angular, injection-dependances, tests, pieges]
---

# DI Angular : tests et erreurs classiques

## Remplacer une dépendance en test

Le code testé fait toujours `inject(ProduitService)`, mais reçoit un faux :

```ts
TestBed.configureTestingModule({
  providers: [
    { provide: ProduitService, useValue: { get: () => of(produitFactice) } }
  ]
});
```

## Erreurs classiques

| Erreur | Cause | Solution |
|---|---|---|
| `NullInjectorError: No provider for X` | Aucun provider atteignable depuis ce point de l'arbre | `providedIn: 'root'` ou provider au bon niveau |
| `NG0203: inject() must be called from an injection context` | `inject()` dans une méthode, callback, `setTimeout` | Injecter en propriété / constructeur |
| `NG0200` dépendance circulaire | A injecte B qui injecte A | Extraire la partie commune dans un 3e service |
| État « partagé » qui ne l'est pas | Service **aussi** listé dans les `providers` d'un composant → instance locale | Retirer le provider local |
| État partagé non voulu | `providedIn: 'root'` alors qu'il fallait une instance par composant | Provider au niveau composant |

## Voir aussi

- [[injection-de-dependances-en-angular]]
- [[angular-di-hierarchie-injecteurs]]
