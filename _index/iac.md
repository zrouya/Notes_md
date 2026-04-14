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

## OpenTofu / Terraform

- [[opentofu-import]] — Import de ressources existantes, modes lifecycle, ARM IDs

## Azure

- [[azure-naming-convention]] — Pattern `ent-az-we-<env>-pfs-<domain>-<type>-<id>`
