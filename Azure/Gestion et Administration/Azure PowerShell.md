
Azure Powershell est un [[Modules Powershell|module]] [[Windows PowerShell|PowerShell]] permettant de se connecter à un Compte [[Azure]] et de **gérer** les ressources Azure correspondantes, via des [[cmdlets PowerShell]] ou des **fichiers de scripts** Powershell.

Il est multi-plateformes, mais sur des environnements autres que Windows il nécessite l'installation de PowerShell (alors que [[Azure CLI]] est exécutable sur [[Bash]]).

Une fois le module [[Installation de Azure Powershell|installé]], on a accès à un ensemble de [[Commandes PowerShell Azure|cmdlets]] Azure, permettant de créer des fichiers de script : 

Exemple : 
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
