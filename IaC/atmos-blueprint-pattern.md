---
tags: [iac, atmos, yaml, pattern, dry]
---

# Atmos — Pattern Blueprint

Encapsuler un groupe de composants liés dans un fichier importable, réutilisable par n'importe quelle stack.

## Structure recommandée

```
stacks/
├── catalog/         # defaults par type de ressource (1 composant)
├── blueprints/      # recettes multi-composants réutilisables
│   └── containerapp-sql-access.yaml
└── templates/       # stacks concrètes
```

## Définition d'un blueprint

Un blueprint n'est qu'un fichier YAML qui importe des catalog defaults et déclare des composants abstraits.

```yaml
# stacks/blueprints/containerapp-sql-access.yaml
import:
  - catalog/sqlAppAccess/_defaults   # définit sqlAppAccess/defaults (abstract)
```

## Utilisation dans une stack concrète

```yaml
import:
  - blueprints/containerapp-sql-access

components:
  terraform:
    sqlAppAccess/ca1-to-db1:
      metadata:
        inherits: [sqlAppAccess/defaults]
      vars:
        app_resource_identifier: "001"
        sql_resource_identifier: "001"
```

## Instanciation multiple du même abstrait

Le composant abstrait est un modèle. La stack crée autant d'instances que nécessaire avec des noms uniques.

```yaml
sqlAppAccess/ca1-to-db1:
  metadata:
    inherits: [sqlAppAccess/defaults]
  vars:
    app_resource_identifier: "001"
    sql_resource_identifier: "001"

sqlAppAccess/ca1-to-db2:
  metadata:
    inherits: [sqlAppAccess/defaults]
  vars:
    app_resource_identifier: "001"
    sql_resource_identifier: "002"
```

Chaque instance a son propre state Terraform. La clé backend est `{{ .component }}/terraform.tfstate`, où `.component` est le nom du composant dans la stack (ex: `sqlAppAccess/ca1-to-db1`). Le slash est valide dans les clés Azure Blob Storage.

## Limite : import non paramétré

Les imports Atmos sont des **merges déclaratifs**, pas des appels paramétrés. On ne peut pas "instancier un blueprint avec des paramètres" — on ne peut qu'importer et hériter.

Conséquence : si un blueprint contient N composants abstraits, une stack qui veut M liens doit écrire M×N blocs concrets.

## Solution : module TF monolithique

Pour avoir **un seul bloc par lien** dans la stack, regrouper toutes les ressources du lien dans un seul module Terraform. Le blueprint n'a alors qu'un seul composant abstrait.

```
Module sqlAppAccess :
  - data "azurerm_container_app"  (lookup IPs sortantes)
  - data "azurerm_mssql_server"   (lookup server ID)
  - azurerm_mssql_firewall_rule   (for_each sur les IPs)
```

Un module de lien ne doit pas créer les ressources liées (Container App, SQL Server) — il les lit via data sources uniquement.

## Voir aussi

- [[atmos-stack-composition]]
- [[azure-containerapp-sql-access]]
