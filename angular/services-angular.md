---
tags: [angular, services]
---

# Services Angular

Classe `@Injectable` qui porte de la **logique** ou de l'**état** réutilisable, indépendamment des composants. Obtenue par [[injection-de-dependances-en-angular|injection de dépendances]].

## Exemple

```ts
@Injectable({ providedIn: 'root' })
export class ProduitService {
  private http = inject(HttpClient);

  get(id: string) {
    return this.http.get<Produit>(`/api/produits/${id}`);
  }
}

// dans un composant
export class DetailComponent {
  private produits = inject(ProduitService);
}
```

## Portée (scope)

| Déclaration | Instance |
|---|---|
| `providedIn: 'root'` | **Singleton** : une seule instance pour toute l'application |
| `providers: [X]` d'un composant | Une instance par instance du composant (et ses enfants) |
| `providers` d'une route | Une instance partagée par la route et ses enfants |

## Usages typiques

- Appels HTTP, accès aux données
- Logique métier partagée
- **État partagé** entre composants sans lien parent/enfant (voir [[etat-partage-service-vs-url]])

## Voir aussi

- [[injection-de-dependances-en-angular]]
- [[etat-partage-service-vs-url]]
- [[angular-signals]]
