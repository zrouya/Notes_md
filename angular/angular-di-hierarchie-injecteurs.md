---
tags: [angular, injection-dependances]
---

# Hiérarchie d'injecteurs Angular

Les injecteurs forment un **arbre** calqué sur l'application. L'endroit où un provider est déclaré détermine **la portée** de l'instance.

## L'arbre

```
platform                 (rare : plusieurs apps sur la page)
  └── root               ← providedIn: 'root', providers de bootstrapApplication
        └── routes       ← providers: [...] d'une route (souvent lazy)
              └── éléments ← providers: [...] de @Component / @Directive
                    AppComponent
                      └── PageComponent
                            └── CarteComponent   ← inject(X) démarre ici
```

## Résolution

1. Injecteur du composant courant
2. Remontée vers les composants **parents**
3. Injecteurs de **routes**, puis **racine**
4. Rien trouvé → `NullInjectorError: No provider for X`

**Le premier provider trouvé en remontant gagne.**

## Portée

| Provider déclaré dans | Instance |
|---|---|
| `providedIn: 'root'` / `bootstrapApplication` | Singleton global |
| `providers` d'une route | Partagée par la route et ses enfants |
| `providers` d'un composant | Une par instance du composant (+ descendants), détruite avec lui |

```ts
@Component({ selector: 'app-editeur', providers: [EditeurStateService] })
export class EditeurComponent {
  state = inject(EditeurStateService);   // état isolé par <app-editeur>
}
```

Usage du provider de composant : état local partagé avec les enfants (formulaire multi-étapes, éditeur, tableau filtrable).

## Modificateurs de recherche

```ts
inject(Logger, { optional: true });   // null au lieu d'une erreur
inject(Logger, { self: true });       // injecteur courant uniquement
inject(Logger, { skipSelf: true });   // commence au parent
inject(Logger, { host: true });       // s'arrête au composant hôte
```

## Voir aussi

- [[injection-de-dependances-en-angular]]
- [[angular-di-providers]]
- [[arbre-composants-vs-arbre-routes]]
