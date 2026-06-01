---
tags: [dotnet, nuget, registry, feed, azure-artifacts, gitlab]
---

# NuGet — Registries (Feeds)

Un registry NuGet est un serveur HTTP exposant l'API NuGet (v2 ou v3). Les clients (`nuget.exe`, `dotnet`) s'y connectent pour rechercher, télécharger et publier des packages.

## nuget.org (registry public officiel)

- URL v3 : `https://api.nuget.org/v3/index.json`
- Héberge tous les packages open-source .NET
- Authentification pour push : API Key générée sur le site
- Immuabilité : un package publié ne peut pas être supprimé (seulement "unlisted")

## Azure Artifacts

Feed privé hébergé dans Azure DevOps.

```
https://pkgs.dev.azure.com/{org}/_packaging/{feed}/nuget/v3/index.json
```

- Authentification : PAT (Personal Access Token) ou credential provider
- Supporte le **upstream caching** : peut proxifier nuget.org et stocker en cache
- Credential provider : `Azure Artifacts Credential Provider` (plugin `nuget-credentialprovider`)

```powershell
# Ajouter le feed
nuget sources add -Name "MonFeed" `
  -Source "https://pkgs.dev.azure.com/org/_packaging/feed/nuget/v3/index.json" `
  -Username "az" -Password "$PAT"

# Push
nuget push MonPackage.1.0.0.nupkg -Source "MonFeed" -ApiKey az
```

## GitHub Packages

Feed privé (ou public) lié à un repo GitHub.

```
https://nuget.pkg.github.com/{owner}/index.json
```

- Authentification : PAT GitHub avec scope `packages:write`
- Un package est lié à un repo GitHub spécifique

```bash
dotnet nuget push MonPackage.1.0.0.nupkg \
  --source "https://nuget.pkg.github.com/owner/index.json" \
  --api-key $GITHUB_TOKEN
```

## GitLab Package Registry

Feed intégré à GitLab, lié à un projet ou un groupe.

```
# Niveau projet
https://gitlab.example.com/api/v4/projects/{id}/packages/nuget/index.json

# Niveau groupe
https://gitlab.example.com/api/v4/groups/{id}/-/packages/nuget/index.json
```

- Authentification : Deploy Token, PAT, ou `CI_JOB_TOKEN` (en CI)
- En CI GitLab, `CI_JOB_TOKEN` permet le push sans credential supplémentaire

```yaml
# .gitlab-ci.yml
publish:
  script:
    - dotnet nuget add source "$CI_SERVER_URL/api/v4/projects/$CI_PROJECT_ID/packages/nuget/index.json"
        --name gitlab --username gitlab-ci-token --password $CI_JOB_TOKEN
    - dotnet nuget push "**/*.nupkg" --source gitlab
```

## `NuGet.config` — Configurer les sources

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org"  value="https://api.nuget.org/v3/index.json" />
    <add key="MonFeed"    value="https://pkgs.dev.azure.com/org/_packaging/feed/nuget/v3/index.json" />
  </packageSources>
  <config>
    <add key="DefaultPushSource" value="https://pkgs.dev.azure.com/org/_packaging/feed/nuget/v3/package" />
  </config>
</configuration>
```

## Self-hosted

Solutions open-source pour héberger son propre feed :
- **BaGet** — léger, open-source, compatible API v3
- **Nexus Repository** — solution entreprise multi-format (NuGet, npm, Docker…)
- **ProGet** — alternative enterprise

## Voir aussi

- [[nuget-overview]]
- [[nuget-creation-package]]
