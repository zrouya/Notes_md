---
tags: [iac, index, moc]
---

# IaC — Map of Content

Notes sur l'Infrastructure as Code : Atmos, OpenTofu/Terraform, Azure.

## Atmos

- [[atmos-overview]] — Vue d'ensemble : structure, commandes de base
- [[atmos-stack-composition]] — Composition des stacks : imports, catalog, mixins, templates
- [[atmos-stack-discovery]] — `included_paths`, `excluded_paths`, `name_template`
- [[atmos-component-function]] — Composants abstraits, `inherits`, `metadata.type`
- [[atmos-blueprint-pattern]] — Pattern blueprint pour stacks multi-composants
- [[atmos-go-templates]] — Go templates dans les manifests : Sprig, gomplate, pièges `.yaml.tmpl`
- [[atmos-datasources]] — Lire des fichiers (JSON, XML) et injecter leur contenu dans des vars
- [[atmos-terraform-output]] — Récupérer les outputs Terraform entre composants
- [[atmos-describe-query]] — `atmos describe stacks` : filtres, requêtes
- [[atmos-describe-dependents]] — Identifier les composants dépendants
- [[atmos-deploy-ordering]] — Ordre de déploiement, dépendances
- [[atmos-ci-dynamic-stack]] — Génération dynamique de stacks en CI

## OpenTofu / Terraform

- [[opentofu-import]] — Import de ressources existantes dans l'état

## Atmos — Authentification Azure

- [[atmos-azure-auth]] — Pipe d'auth complet, provider kinds, pièges MSAL et conflit subscription/tenant
- [[atmos-azure-backend]] — Backend azurerm : `use_azuread_auth`, RBAC requis, provisionnement auto du container
- [[azure-oidc-workload-identity]] — Workload Identity Federation GitLab→Azure : flux, config Atmos, pipeline sans secret

## Azure

- [[azure-containerapp-sql-access]] — Accès SQL depuis un Container App
