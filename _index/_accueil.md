---
tags: [index, moc, accueil]
---

# Accueil — sommaire des index

Point d'entrée du vault. Chaque domaine a son *Map of Content* qui liste ses notes.

## Infrastructure & réseau

| Index | Contenu |
|-------|---------|
| [[_index/reseau\|Réseau]] | OSI, adressage, DNS, TCP/UDP, HTTP, diagnostic |
| [[_index/tls\|TLS / HTTPS]] | Handshakes, certificats, chaîne de confiance |
| [[_index/azure\|Azure]] | APIM, Entra, managed identities, monitoring |
| [[_index/observabilite\|Observabilité]] | Piliers, OpenTelemetry, Grafana, Tempo, APM |
| [[_index/iac\|IaC]] | Terraform, Atmos, backends |

## Développement

| Index | Contenu |
|-------|---------|
| [[_index/dotnet\|.NET / NuGet]] | Écosystème .NET |
| [[_index/poo\|POO & Conception]] | Design patterns, principes |
| [[_index/graphql\|GraphQL]] | Schéma, résolveurs |
| [[_index/cache\|Cache]] | Stratégies d'invalidation |

## Outillage

| Index | Contenu |
|-------|---------|
| [[_index/bash\|Bash]] | Syntaxe, options, patterns |
| [[_index/git\|Git]] | Commandes et workflows |
| [[_index/gitlab-ci\|GitLab CI]] | Jobs, variables, pipelines |
| [[_index/outils\|Outils]] | yq, openssl, profilers, CLI |
| [[_index/mcp\|MCP]] | Model Context Protocol |

## Convention du vault

- Un **dossier par domaine** (`reseau/`, `tls/`, `azure/`, `bash/`, `gitlab-ci/`…), chacun avec son index dans `_index/`.
- Deux styles de noms y cohabitent : `kebab-case` pour les notes récentes, `Titre Long` pour les notes historiques. Les liens Obsidian se résolvant **par nom de fichier et non par chemin**, un déplacement de note ne casse aucun lien.
- Le reste de la racine attend d'être classé par domaine — `reseau/` a été consolidé le 2026-09-03, `observabilite/` le 2026-09-14 ; les notes Docker, .NET et Angular sont encore à la racine.
