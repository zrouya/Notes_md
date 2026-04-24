---
tags: [azure, container-app, sql-server, managed-identity, iac]
---

# Azure — Accès Container App → SQL Server

Trois couches à configurer pour qu'une Container App puisse interroger un SQL Server.

## Vue d'ensemble

| Couche | Ressource Azure | Outil | Status |
|---|---|---|---|
| Réseau | `azurerm_mssql_firewall_rule` (une par IP sortante) | Terraform | ✅ gérable |
| Authentification | Managed Identity sur la Container App | Terraform (`identity {}`) | ✅ gérable |
| Authentification | Entra admin sur le SQL Server | Terraform | ✅ gérable |
| Autorisation | Contained user dans la DB | T-SQL | ⚠️ hors TF |

## Mode SQL Authentication (login/password)

- Firewall rules pour les IPs sortantes de la Container App
- Créer un login SQL + user + rôles via T-SQL (hors TF)
- Passer les credentials à l'app (env var / Key Vault)

## Mode Managed Identity (recommandé)

### 1. Firewall rules — module `sqlAppAccess`

```hcl
data "azurerm_container_app" "app" { ... }

resource "azurerm_mssql_firewall_rule" "app_access" {
  for_each         = toset(data.azurerm_container_app.app.outbound_ip_addresses)
  name             = "allow-app-${replace(each.value, ".", "-")}"
  server_id        = data.azurerm_mssql_server.server.id
  start_ip_address = each.value
  end_ip_address   = each.value
}
```

### 2. Managed Identity — module `containerApp`

```hcl
dynamic "identity" {
  for_each = var.identity != null ? [var.identity] : []
  content {
    type         = identity.value.type   # "SystemAssigned" | "UserAssigned" | "SystemAssigned, UserAssigned"
    identity_ids = length(identity.value.identity_ids) > 0 ? identity.value.identity_ids : null
  }
}
```

Variable Atmos à passer dans la stack :
```yaml
identity:
  type: SystemAssigned
```

### 3. Entra admin sur SQL Server — module `sqlServer`

```hcl
resource "azurerm_mssql_server_active_directory_administrator" "entra_admin" {
  count          = var.entra_admin != null ? 1 : 0
  server_id      = azurerm_mssql_server.sql_server[0].id
  login_username = var.entra_admin.login_username
  object_id      = var.entra_admin.object_id
  tenant_id      = var.entra_admin.tenant_id
}
```

### 4. Contained user — T-SQL (hors TF)

À exécuter après déploiement (script CI ou init applicatif) :

```sql
-- Dans le contexte de la base cible
CREATE USER [nom-managed-identity] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [nom-managed-identity];
ALTER ROLE db_datawriter ADD MEMBER [nom-managed-identity];
```

## Ce qui appartient à quel module

- `sqlAppAccess` → règles de pare-feu (lit CA et SQL Server en data source)
- `containerApp` → bloc `identity` (la MI est une propriété de l'app)
- `sqlServer` → `entra_admin` optionnel (auth Entra au niveau serveur)
- T-SQL → droits data-plane dans la base (hors Terraform)

## Voir aussi

- [[atmos-blueprint-pattern]]
