---
tags: [index, moc, accueil]
---

# Accueil — sommaire des index

Point d'entrée du vault. Chaque domaine a son *Map of Content* qui liste ses notes.

## Infrastructure & réseau

| Index | Contenu |
|-------|---------|
| [[_index-reseau\|Réseau]] | OSI, adressage, DNS, TCP/UDP, HTTP, diagnostic |
| [[_index-tls\|TLS / HTTPS]] | Handshakes, certificats, chaîne de confiance |
| [[_index-azure\|Azure]] | APIM, Entra, managed identities, monitoring, App Service, Functions, VM, stockage |
| [[_index-observabilite\|Observabilité]] | Piliers, OpenTelemetry, Grafana, Tempo, APM |
| [[_index-iac\|IaC]] | Terraform, Atmos, backends, ARM Templates |
| [[_index-docker\|Docker]] | Images, volumes, réseau, Compose, Swarm |
| [[_index-kubernetes\|Kubernetes]] | Concepts et clusters |
| [[_index-linux\|Linux]] | Namespaces, cgroups, primitives noyau |
| [[_index-windows-iis\|Windows / IIS]] | IIS, Windows Server, Web Deploy |

## Développement

| Index | Contenu |
|-------|---------|
| [[_index-dotnet\|.NET / NuGet]] | Écosystème .NET, ASP.NET Core |
| [[_index-poo\|POO & Conception]] | Design patterns, principes |
| [[_index-graphql\|GraphQL]] | Schéma, résolveurs |
| [[_index-cache\|Cache]] | Stratégies d'invalidation |
| [[_index-angular\|Angular]] | Composants, routing, data binding, TypeScript |
| [[_index-securite\|Sécurité applicative]] | CSRF, OAuth/OpenID, SOP, anti-forgery |

## Outillage

| Index | Contenu |
|-------|---------|
| [[_index-bash\|Bash]] | Syntaxe, options, patterns |
| [[_index-powershell\|PowerShell]] | Modules, cmdlets |
| [[_index-git\|Git]] | Commandes et workflows |
| [[_index-gitlab-ci\|GitLab CI]] | Jobs, variables, pipelines |
| [[_index-outils\|Outils]] | yq, openssl, profilers, CLI |
| [[_index-mcp\|MCP]] | Model Context Protocol |

## Personnel

| Index | Contenu |
|-------|---------|
| [[_index-perso\|Perso]] | CV, administratif, suivi d'objectifs |

## Convention du vault

- Un **dossier par domaine** (`reseau/`, `tls/`, `azure/`, `bash/`, `gitlab-ci/`…), chacun avec son index dans `_index/`.
- Deux styles de noms y cohabitent : `kebab-case` pour les notes récentes, `Titre Long` pour les notes historiques. Les liens Obsidian se résolvant **par nom de fichier et non par chemin**, un déplacement de note ne casse aucun lien.
- La racine a été classée par domaine le 2026-09-23 (Angular, Docker, .NET, Azure PaaS, etc.) ; le renommage complet en kebab-case reste un chantier progressif, note par note.
