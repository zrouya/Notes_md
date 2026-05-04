---
tags: [graphql, dotnet, hot-chocolate, aspnetcore]
---

# Hot Chocolate

Bibliothèque GraphQL pour ASP.NET Core. La plus complète de l'écosystème .NET, avec intégration native Entity Framework et support du code-first.

## Installation

```bash
dotnet add package HotChocolate.AspNetCore
dotnet add package HotChocolate.Data.EntityFramework
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## Câblage dans Program.cs

```csharp
builder.Services
    .AddDbContext<AppDbContext>(opt =>
        opt.UseSqlServer(connectionString))
    .AddGraphQLServer()
    .AddQueryType<Query>()
    .AddType<ProductType>()
    .AddDataLoader<CategoryBatchDataLoader>()
    .AddFiltering()
    .AddSorting()
    .AddProjections();

app.MapGraphQL(); // expose POST /graphql
```

## Définition d'un type (Code-first)

```csharp
public class ProductType : ObjectType<Product>
{
    protected override void Configure(IObjectTypeDescriptor<Product> descriptor)
    {
        descriptor.Field(p => p.CategoryId).Ignore();
        descriptor.Field(p => p.Category)
            .ResolveWith<ProductResolvers>(r =>
                r.GetCategoryAsync(default!, default!));
    }
}
```

## Query avec pagination / filtres / tri

```csharp
public class Query
{
    [UseDbContext(typeof(AppDbContext))]
    [UsePaging]
    [UseFiltering]
    [UseSorting]
    public IQueryable<Product> GetProducts([ScopedService] AppDbContext db)
        => db.Products;
}
```

## Points d'attention

- Utiliser `IDbContextFactory` dans les DataLoaders (pas `IDbContext` scopé)
- Activer `AddProjections()` pour que EF ne charge que les colonnes demandées
- En prod : désactiver l'introspection et activer les persisted queries

## Voir aussi

- [[GraphQL]]
- [[GraphQL DataLoader]]
- [[ASP.NET Core]]
