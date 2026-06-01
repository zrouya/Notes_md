---
tags: [dotnet, nuget, packaging]
---

# Créer un package NuGet

Deux grandes ères dans la création de packages NuGet.

## Approche historique — `.nuspec` + `nuget.exe` (pre-2017)

Utilisée avec les projets `.csproj` "classiques" (non-SDK).

**1. Créer un `.nuspec` manuellement :**
```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/10/nuspec.xsd">
  <metadata>
    <id>MonPackage</id>
    <version>1.0.0</version>
    <authors>Auteur</authors>
    <description>Description</description>
    <dependencies>
      <dependency id="Newtonsoft.Json" version="10.0.3" />
    </dependencies>
  </metadata>
  <files>
    <file src="bin\Release\MonPackage.dll" target="lib\net46" />
  </files>
</package>
```

**2. Builder et packer :**
```powershell
msbuild MonProjet.csproj /p:Configuration=Release
nuget pack MonPackage.nuspec
```

**3. Pusher :**
```powershell
nuget push MonPackage.1.0.0.nupkg -Source https://mon-feed/
```

## Approche moderne — SDK-style (depuis .NET Core / .NET 5+)

Les projets SDK-style (`<Project Sdk="Microsoft.NET.Sdk">`) embarquent les métadonnées directement dans le `.csproj`. Plus besoin de `.nuspec` séparé dans la majorité des cas.

**`.csproj` :**
```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <PackageId>MonPackage</PackageId>
    <Version>1.0.0</Version>
    <Authors>Auteur</Authors>
    <Description>Description</Description>
    <IsPackable>true</IsPackable>
  </PropertyGroup>
</Project>
```

**Builder + packer en une commande :**
```bash
dotnet pack --configuration Release
```

**Pusher :**
```bash
dotnet nuget push bin/Release/MonPackage.1.0.0.nupkg \
  --source https://mon-feed/ \
  --api-key $API_KEY
```

## Comparaison

| | Historique `.nuspec` | SDK-style |
|---|---|---|
| Fichier manifeste | `.nuspec` séparé | Inline dans `.csproj` |
| Commande pack | `nuget pack` | `dotnet pack` |
| Framework cible | `net46`, `net472`… | `net8.0`, `netstandard2.0`… |
| Multiframework | Manuel (sections `<files>`) | `<TargetFrameworks>` |
| État | Legacy | Recommandé |

## Voir aussi

- [[nuget-overview]]
- [[nuget-versioning]]
- [[nuget-registries]]
