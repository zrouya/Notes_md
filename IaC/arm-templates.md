---
tags: [iac, azure, arm]
---

# ARM Templates

Les Templates ARM sont des **fichiers json** permettant d'**automatiser** auprès d'[[azure-resource-manager-arm|ARM]] la création et la gestion de [[resource-azure|ressources]] Azure.

## Structure

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {}, //<- json object expected
    "functions": [], //<- array expected
    "variables": {},
    "resources": [],
    "outputs": {}
}
```

Exemple, pour la simple création d'un storage account :

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {           
            "name": "templatestore1000",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "Central US",
            "apiVersion": "2021-06-01",
            "sku" :{"name": "Standard_LRS"},
            "kind": "StorageV2"
        }
            ]
}
```

## Déploiement

Une fois le fichier créé, il suffit de **déployer** les **ressources** correspondantes dans Azure.

Par exemple :
- Avec [[azure-powershell]] (a exécuter à l'emplacement du fichier template.json):

```powershell
Connect-AzAccount

New-AzResourceGroupDeployment -ResourceGroupName myGroup `
-TemplateFile template.json
```

- Via le **portail Azure**, il suffit de créer une ressource de type **Custom deployment**
	- "**Build your own template in the editor**" -> copier coller le template dans le champ, ou uploader le fichier de template
	- Ajouter un **resource group** existant, ou en créer un nouveau

Note : lors de la création d'un custom deployment, le portail Azure propose également des templates types permettant de créer des ressources complexes, comme les [[creation-vm-azure-via-arm-templates|machines virtuelles]].

## Voir aussi

- [[azure-resource-manager-arm]]
- [[creation-vm-azure-via-arm-templates]]
- [[creer-copies-ressources-arm-template]]
