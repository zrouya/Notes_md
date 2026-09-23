---
tags: [azure, powershell, administration]
---

# Azure PowerShell

Azure PowerShell est un [[modules-powershell|module]] [[windows-powershell|PowerShell]] permettant de se connecter à un compte [[azure-presentation|Azure]] et de **gérer** les ressources Azure correspondantes, via des [[cmdlets-powershell]] ou des **fichiers de scripts** PowerShell.

Il est multi-plateformes, mais sur des environnements autres que Windows il nécessite l'installation de PowerShell (alors que [[azure-cli|Azure CLI]] est exécutable sur [[bash-overview]]).

## Exemples de scripts

Une fois le module [[installation-azure-powershell|installé]], on a accès à un ensemble de [[commandes-powershell-azure|cmdlets]] Azure, permettant de créer des fichiers de script :

```powershell
$prop = @{
    Location          = "West US"
    Properties        = @{test = "test"}
    ResourceName      = "TestSite06"
    ResourceType      = "microsoft.web/sites"
    ResourceGroupName = "ResourceGroup11"
    Force             = $true
}

New-AzResource @prop
```

ou

``` powershell
$ResourceGroupName="powershell-grp"
$Location="North Europe"
$AppServicePlanName="companyplan"
$WebAppName="companyapp10000"

Connect-AzAccount

New-AzResouceGroup -Name $ResourceGroupName -Location $Location

# We first need to create an App Service Plan

New-AzAppServicePlan -ResourceGroupName $ResourceGroupName `
-Location $Location -Tier "B1" -NumberofWorkers 1 -Name $AppServicePlanName

# Then we can create the Azure Web App

New-AzWebApp -ResourceGroupName $ResourceGroupName -Name $WebAppName `
-Location $Location -AppServicePlan $AppServicePlanName
```

## Voir aussi

- [[installation-azure-powershell]]
- [[commandes-powershell-azure]]
- [[azure-cli]]
