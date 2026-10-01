---
tags: [angular, injection-dependances]
---

# Injection de dépendances en Angular

Une classe **déclare** ses dépendances au lieu de les fabriquer ; un **injecteur** Angular les crée, les met en cache et les lui fournit (inversion de contrôle).

## Exemple

```ts
// ❌ couplage fort : la classe fabrique sa dépendance
private http = new HttpClient(new HttpXhrBackend(...));

// ✅ la classe demande, l'injecteur fournit
@Injectable({ providedIn: 'root' })
export class ProduitService {
  private http = inject(HttpClient);
}
```

## Les trois acteurs

| Acteur | Rôle | Exemple |
|---|---|---|
| **Token** | La clé demandée | `ProduitService`, `InjectionToken` |
| **Provider** | La recette pour créer la valeur | `providedIn: 'root'`, `{ provide, useClass }` |
| **Injecteur** | Associe tokens et recettes, crée et cache les instances | racine, route, composant |

## `inject()` vs constructeur

```ts
private route = inject(ActivatedRoute);                  // moderne, recommandé
constructor(private route: ActivatedRoute) {}            // classique
```

`inject()` fonctionne aussi dans des **fonctions** (guards, resolvers, intercepteurs), mais seulement dans un **contexte d'injection** :

```ts
private a = inject(A);           // ✅ initialisation de propriété
constructor() { inject(B); }     // ✅ constructeur
onClick() { inject(C); }         // ❌ NG0203
```

## Intérêts

- Découplage : changer d'implémentation sans toucher aux consommateurs
- Tests : remplacer un service par un mock
- Partage d'instance → état partagé ([[etat-partage-service-vs-url]])
- Cycle de vie géré par Angular

## Voir aussi

- [[angular-di-providers]] — recettes, `InjectionToken`, `multi`
- [[angular-di-hierarchie-injecteurs]] — arbre, résolution, portée
- [[angular-di-tests-erreurs]]
- [[services-angular]]
