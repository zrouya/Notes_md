---
tags: [dotnet, nuget, versioning, semver]
---

# NuGet — Versioning

NuGet suit **SemVer 2.0.0** pour le versioning des packages.

## Format SemVer

```
MAJOR.MINOR.PATCH[-prerelease][+buildmetadata]
```

| Segment | Signification |
|---|---|
| `MAJOR` | Breaking change |
| `MINOR` | Nouvelle fonctionnalité rétrocompatible |
| `PATCH` | Bug fix rétrocompatible |
| `prerelease` | Version instable (alpha, beta, rc, ci…) |

## Versions prérelease

Les versions prérelease sont **inférieures** à la release stable correspondante.

```
1.0.0-alpha < 1.0.0-beta < 1.0.0-rc.1 < 1.0.0
```

Exemples courants :
```
1.2.0-alpha
1.2.0-beta.1
1.2.0-rc.2
1.2.0-ci-20260526-143000   ← suffixe timestamp (CI)
```

Les clients NuGet n'installent **pas** les prérelease par défaut — il faut l'activer explicitement (`--prerelease` ou cocher "Inclure les préversions" dans Visual Studio).

## Floating versions (PackageReference uniquement)

Permet de cibler dynamiquement la dernière version correspondant à un pattern :

```xml
<PackageReference Include="MonPackage" Version="1.*" />        <!-- dernière 1.x -->
<PackageReference Include="MonPackage" Version="1.2.*" />      <!-- dernière 1.2.x -->
<PackageReference Include="MonPackage" Version="*" />          <!-- absolument dernière -->
<PackageReference Include="MonPackage" Version="*-*" />        <!-- dernière incl. prérelease -->
```

Déconseillé en production (builds non reproductibles).

## Ranges de version

NuGet supporte les intervalles dans les déclarations de dépendances (`.nuspec` / `PackageReference`) :

```
[1.0.0]          exactement 1.0.0
[1.0.0, 2.0.0)   ≥ 1.0.0 et < 2.0.0
(1.0.0, )        > 1.0.0
```

## Bonnes pratiques

- En production : version **exacte** ou range fermé (`[x.y.z, x+1.0.0)`)
- En CI : suffixe timestamp pour traçabilité (`1.0.0-ci-20260526-143000`)
- En release : version stable sans suffixe (`1.0.0`)
- Bumper le MAJOR à chaque breaking change pour signaler aux consommateurs

## Voir aussi

- [[nuget-overview]]
- [[nuget-creation-package]]
- [[nuget-gestion-dependances]]
