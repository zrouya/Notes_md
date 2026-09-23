---
tags: [azure, powershell, github]
---

# Azure PowerShell - Déploiements via GitHub

Pour configurer un [[publication-azure-web-app-via-github|déploiement]] manuel par GitHub d'une [[azure-web-application|webApp]] Azure via un script PowerShell :

```powershell
# We are deploying an application from GitHub onto an existing Web App

Connect-AzAccount

$ResourceGroupName="powershell-grp"
$WebAppName="companyapp10000"
$Properties =@{

    repoUrl="https://github.com/gitHubAccount/repositoryName";

    branch="master";

    isManualIntegration="true";

}

Set-AzResource -ResourceGroupName $ResourceGroupName `
-Properties $Properties -ResourceType Microsoft.Web/sites/sourcecontrols `
-ResourceName $WebAppName/web -ApiVersion 2015-08-01 -Force
```

## Voir aussi

- [[publication-azure-web-app-via-github]]
- [[azure-powershell]]
