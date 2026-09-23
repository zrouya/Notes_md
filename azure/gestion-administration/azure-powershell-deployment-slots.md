---
tags: [azure, powershell, deployment-slots]
---

# Azure PowerShell - Deployment Slots

Pour gérer les [[azure-deployment-slots|emplacements de déploiement]] Azure via PowerShell, et pour [[swap-deployment-slots-azure|swapper]] les deployment slots Azure :

```powershell
$ResourceGroupName="powershell-grp"
$WebAppName="companyapp10000"
$AppServicePlanName="companyplan"

Connect-AzAccount

# For deployment slots, the App Service Plan needs to be standard or higher
# Here we set the AppServicePlan Tier to 'Standard'
Set-AzAppServicePlan -Name $AppServicePlanName -ResourceGroupName ` $ResourceGroupName -Tier Standard

# We then create a Web App slot
$SlotName="Staging"
New-AzWebAppSlot -Name $WebAppName -ResourceGroupName $ResourceGroupName `
-Slot $SlotName

# We then deploy an application onto the Staging slot

$Properties =@{
    repoUrl="";
    branch="master";
    isManualIntegration="true";
}

Set-AzResource -ResourceGroupName $ResourceGroupName `
-Properties $Properties -ResourceType Microsoft.Web/sites/slots/sourcecontrols `
-ResourceName $WebAppName/$SlotName/web -ApiVersion 2015-08-01 -Force

# The following can be used to switch slots
$TargetSlot="production"
Switch-AzWebAppSlot -Name $WebAppName -ResourceGroupName $ResourceGroupName `
-SourceSlotName $SlotName -DestinationSlotName $TargetSlot
```

## Voir aussi

- [[azure-deployment-slots]]
- [[swap-deployment-slots-azure]]
- [[azure-powershell]]
