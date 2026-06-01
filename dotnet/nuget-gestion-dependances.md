---
tags: [dotnet, nuget, dependances]
---

# NuGet — Gestion des dépendances

Deux systèmes ont coexisté historiquement pour déclarer les dépendances NuGet dans un projet.

## `packages.config` (legacy — .NET Framework)

Fichier XML à la racine de chaque projet. Les packages sont copiés dans un dossier `packages/` à la racine de la solution.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Newtonsoft.Json" version="10.0.3" targetFramework="net46" />
  <package id="EntityFramework"  version="6.1.3"  targetFramework="net46" />
</packages>
```

- Chaque projet a **son propre** `packages.config`
- Les versions sont **figées** (pas de floating versions)
- Dépendances transitives : **toutes listées explicitement** (bruit important)
- Restauration : `nuget restore MonProjet.sln`

## `PackageReference` (moderne — SDK-style et .NET Framework ≥ 4.x)

Les dépendances sont déclarées directement dans le `.csproj`. Pas de fichier `packages.config`.

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="10.0.3" />
  <PackageReference Include="EntityFramework"  Version="6.1.3"  />
</ItemGroup>
```

- Les dépendances transitives sont **résolues automatiquement** et non listées
- Restauration : `dotnet restore` ou `nuget restore`
- Les packages vont dans le cache global (`%USERPROFILE%\.nuget\packages\`) plutôt que dans un dossier `packages/` local
- Supporte les **floating versions** : `Version="10.*"` prend la dernière 10.x

## Migration `packages.config` → `PackageReference`

Visual Studio propose une migration automatique via clic droit sur `packages.config` → *Migrer packages.config vers PackageReference*.

## `central package management` (nuget 6.2+)

Centralise toutes les versions dans un fichier `Directory.Packages.props` à la racine de la solution :

```xml
<Project>
  <ItemGroup>
    <PackageVersion Include="Newtonsoft.Json" Version="13.0.3" />
  </ItemGroup>
</Project>
```

Dans les `.csproj`, on référence sans version :
```xml
<PackageReference Include="Newtonsoft.Json" />
```

## Voir aussi

- [[nuget-overview]]
- [[nuget-dependances-transitives]]
- [[nuget-versioning]]
