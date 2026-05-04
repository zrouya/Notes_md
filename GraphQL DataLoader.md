---
tags: [graphql, performance, dataloader, n+1]
---

# GraphQL DataLoader

Mécanisme qui résout le problème N+1 en **regroupant** (batching) les appels individuels aux resolvers en une seule requête, et en les **mettant en cache** pour la durée d'une requête GraphQL.

## Le problème N+1

Sans DataLoader, résoudre un champ imbriqué pour N parents génère N+1 requêtes :

```
SELECT * FROM Products                     → 1 requête, 100 lignes
SELECT * FROM Categories WHERE Id = 3     → 1 requête par produit
SELECT * FROM Categories WHERE Id = 7     → ...
...                                        → 101 requêtes au total ❌
```

## Comment fonctionne le DataLoader

```
Resolver produit 1  ──┐
Resolver produit 2  ──┤  COLLECT — accumule les IDs (même tick)
Resolver produit 3  ──┘
                        │
                        ▼
          BATCH : WHERE Id IN (3, 7, 12, ...)  → 1 seule requête ✅
                        │
          Redistribution de chaque résultat au bon resolver
```

- **Batching** : tous les IDs du même niveau d'exécution sont groupés
- **Caching** : si deux parents partagent la même clé, elle n'est résolue qu'une fois

## Implémentation (Hot Chocolate / .NET)

```csharp
// 1 clé → 1 résultat
public class CategoryBatchDataLoader : BatchDataLoader<int, Category>
{
    private readonly IDbContextFactory<AppDbContext> _dbFactory;

    public CategoryBatchDataLoader(IDbContextFactory<AppDbContext> dbFactory,
        IBatchScheduler scheduler, DataLoaderOptions options)
        : base(scheduler, options) => _dbFactory = dbFactory;

    protected override async Task<IReadOnlyDictionary<int, Category>> LoadBatchAsync(
        IReadOnlyList<int> keys, CancellationToken ct)
    {
        await using var db = await _dbFactory.CreateDbContextAsync(ct);
        return await db.Categories
            .Where(c => keys.Contains(c.Id))
            .ToDictionaryAsync(c => c.Id, ct);
    }
}
```

Injection dans le resolver :

```csharp
public async Task<Category> GetCategoryAsync(
    [Parent] Product product,
    CategoryBatchDataLoader loader,
    CancellationToken ct)
    => await loader.LoadAsync(product.CategoryId, ct);
```

## Types de DataLoader (Hot Chocolate)

| Type | Usage |
|------|-------|
| `BatchDataLoader<TKey, TValue>` | 1 clé → 1 résultat (ex: Category par Id) |
| `GroupedDataLoader<TKey, TValue>` | 1 clé → N résultats (ex: Products par CategoryId) |
| `CacheDataLoader<TKey, TValue>` | Cache uniquement, pas de batch |

## Voir aussi

- [[GraphQL]]
- [[Hot Chocolate]]
