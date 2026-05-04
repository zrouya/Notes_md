---
tags: [graphql, api, backend]
---

# GraphQL

Langage de requête pour APIs (créé par Meta) qui permet aux clients de demander **exactement** les données dont ils ont besoin — ni plus, ni moins. Contrairement à REST, le client contrôle la forme de la réponse.

## Concepts clés

- **Schema** : contrat entre client et serveur, écrit en SDL
- **Resolvers** : fonctions côté serveur qui "résolvent" chaque champ
- **Query** : lecture de données (≈ GET)
- **Mutation** : écriture de données (≈ POST/PUT/DELETE)
- **Subscription** : flux temps réel (WebSocket)

## Exemple de requête client

```graphql
query {
  product(id: "42") {
    name
    price
    category {
      label
    }
  }
}
```

## Avantages vs REST

| REST | GraphQL |
|------|---------|
| Endpoints multiples | Un seul endpoint `POST /graphql` |
| Structure de réponse fixe | Client choisit les champs |
| Over/under-fetching fréquent | Données exactes |
| Documentation séparée | Schéma = documentation + introspection |

## Idéal pour

- Clients aux besoins hétérogènes (mobile vs web vs partenaires)
- Agréger plusieurs sources de données
- APIs publiques ou multi-consommateurs

## Voir aussi

- [[GraphQL DataLoader]]
- [[Hot Chocolate]]
