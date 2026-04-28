---
tags: [index, iac]
---

# Index — IaC (Infrastructure as Code)

Atmos, OpenTofu, Azure.

## Atmos

- [[atmos-overview]] — Vue d'ensemble, commandes, configuration atmos.yaml
- [[atmos-stack-composition]] — Architecture mixins / catalog / templates, héritage de composants
- [[atmos-stack-discovery]] — included_paths, excluded_paths, name_template, project-config pattern
- [[atmos-go-templates]] — Rendu single-pass, ce qui fonctionne / ne fonctionne pas, .yaml.tmpl
- [[atmos-component-function]] — `atmos.Component` pour accès cross-composant sans déploiement
- [[atmos-terraform-output]] — `!terraform.output` pour outputs Terraform, ordre de déploiement

- [[atmos-blueprint-pattern]] — Blueprint pattern : dossier blueprints/, instanciation multiple, limite import non paramétré, module monolithique de lien
- [[atmos-describe-query]] — Filtrer la sortie de `describe stacks` avec des expressions yq, lister les composants concrets
- [[atmos-describe-dependents]] — `describe dependents`, `settings.depends_on`, limitation : déclaration explicite requise
- [[atmos-ci-dynamic-stack]] — Pattern CI : assemblage dynamique d'un fichier stack depuis un config projet, deep merge yq
- [[atmos-deploy-ordering]] — 3 stratégies d'ordonnancement des composants (deploy_order, Workflows, depends_on)

## OpenTofu / Terraform

- [[opentofu-import]] — Import de ressources existantes, modes lifecycle, ARM IDs

## Azure

- [[azure-containerapp-sql-access]] — Couches réseau / auth / authz pour Container App → SQL Server, ce qui est TF vs T-SQL
