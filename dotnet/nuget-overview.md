---
tags: [dotnet, nuget, package-manager]
---

# NuGet — Vue d'ensemble

Gestionnaire de packages officiel pour l'écosystème .NET. Permet de distribuer, consommer et versionner des bibliothèques.

## Concepts fondamentaux

- **Package** : archive `.nupkg` (zip) contenant des DLLs, assets, et un manifeste `.nuspec`
- **Feed / Registry** : serveur HTTP exposant une API NuGet (v2 ou v3) qui liste et sert les packages
- **Client NuGet** : `nuget.exe`, `dotnet` CLI, ou Visual Studio — ils résolvent et téléchargent les packages
- **Restore** : opération qui télécharge tous les packages déclarés dans le projet

## Flux général

```
Développeur → nuget pack → .nupkg → nuget push → Feed (registry)
                                                       ↓
Consommateur ← nuget restore ← résolution des dépendances
```

## Fichiers clés

| Fichier | Rôle |
|---|---|
| `.nuspec` | Manifeste du package (id, version, dépendances, fichiers inclus) |
| `packages.config` | Liste des dépendances (ancienne approche) |
| `.csproj` avec `<PackageReference>` | Liste des dépendances (approche moderne SDK-style) |
| `NuGet.config` | Configuration des sources, credentials, push target |
| `*.nupkg` | Archive du package à distribuer |

## Voir aussi

- [[nuget-creation-package]]
- [[nuget-gestion-dependances]]
- [[nuget-versioning]]
- [[nuget-registries]]
- [[nuget-dependances-transitives]]
