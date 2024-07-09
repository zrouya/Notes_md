
Les commandes Azure PowerShell sont classées par [[Modules Powershell|modules]].

| Resource type                                                                                            | Azure PowerShell module                                                                       | Noun prefix        |
| -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ------------------ |
| [Resource group](https://learn.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) | [Az.Resources](https://learn.microsoft.com/en-us/powershell/module/az.resources#resources)    | `AzResourceGroup`  |
| [Virtual machines](https://learn.microsoft.com/en-us/azure/virtual-machines)                             | [Az.Compute](https://learn.microsoft.com/en-us/powershell/module/az.compute#virtual_machines) | `AzVM`             |
| [Storage accounts](https://learn.microsoft.com/en-us/azure/storage/common/storage-introduction)          | [Az.Storage](https://learn.microsoft.com/en-us/powershell/module/az.storage/)                 | `AzStorageAccount` |
| [Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/key-vault-whatis)                          | [Az.KeyVault](https://learn.microsoft.com/en-us/powershell/module/az.keyvault)                | `AzKeyVault`       |
| [Web applications](https://learn.microsoft.com/en-us/azure/app-service)                                  | [Az.Websites](https://learn.microsoft.com/en-us/powershell/module/az.websites)                | `AzWebApp`         |
| [SQL databases](https://learn.microsoft.com/en-us/azure/sql-database)                                    | [Az.Sql](https://learn.microsoft.com/en-us/powershell/module/az.sql)                          | `AzSqlDatabase`    |

Pour **se connecter** à un **compte Azure** à partir de **PowerShell** la commande ``Connect-AzAccount`` permet plusieurs méthodes d'authentification. Voir https://learn.microsoft.com/fr-fr/powershell/azure/authenticate-azureps?view=azps-11.5.0&viewFallbackFrom=azps-11.1.0.

Voir la documentation Microsoft : 
https://learn.microsoft.com/en-us/powershell/azure/get-started-azureps?view=azps-11.5.0.
