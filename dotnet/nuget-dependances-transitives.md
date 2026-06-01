---
tags: [dotnet, nuget, dependances, transitives]
---

# NuGet — Dépendances transitives

Une dépendance transitive est une dépendance d'une dépendance : si mon projet dépend de `A`, et que `A` dépend de `B`, alors `B` est une dépendance transitive de mon projet.

## Comportement selon le système

### `packages.config` (legacy)

Toutes les dépendances transitives sont **listées explicitement** dans chaque `packages.config`. C'est NuGet (ou Visual Studio) qui les ajoute automatiquement à l'installation, mais elles restent visibles et figées.

→ Verbeux, mais 100% explicite.

### `PackageReference` (moderne)

Les dépendances transitives sont **résolues automatiquement** au restore et ne sont **pas listées** dans le `.csproj`. Seules les dépendances directes apparaissent.

→ Lisible, mais le graphe complet n'est visible que via commandes.

## Inspecter le graphe de dépendances

```bash
# Afficher toutes les dépendances (directes + transitives)
dotnet list package --include-transitive

# Format arbre avec versions résolues
dotnet list package --include-transitive --format json
```

## Résolution de conflits de versions

Quand deux dépendances requièrent des versions différentes d'un même package, NuGet applique la règle **"lowest applicable version"** : il prend la version minimale satisfaisant toutes les contraintes.

Pour forcer une version spécifique, déclarer la dépendance **directement** dans le projet :
```xml
<!-- Force Newtonsoft.Json en 13.0.3 même si une dep transitive veut 10.x -->
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
```

## `PrivateAssets` — Contrôler la propagation

Par défaut, les dépendances d'un package sont propagées aux consommateurs. On peut restreindre ça :

```xml
<!-- La dépendance n'est pas propagée aux consommateurs du package -->
<PackageReference Include="Moq" Version="4.20.0">
  <PrivateAssets>all</PrivateAssets>
</PackageReference>
```

Valeurs : `all`, `none`, `compile`, `runtime`, `contentfiles`, `analyzers`, `build`, `native`

Utile pour : outils de dev, analyzers, packages de test — qui ne doivent pas "polluer" les consommateurs.

## Voir aussi

- [[nuget-overview]]
- [[nuget-gestion-dependances]]
