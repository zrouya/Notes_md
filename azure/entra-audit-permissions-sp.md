---
tags: [azure, entra, identity, iam, graph, azure-cli]
---

# Entra — Auditer les permissions d'un service principal

Vérifier ce qu'un compte de service détient réellement. Toujours regarder les **deux** référentiels : permissions Graph et rôles d'annuaire ([[entra-permissions-vs-roles]]).

## Portail

| Ce qu'on cherche | Où |
|---|---|
| Permissions **accordées** | Enterprise applications → l'app → **Permissions** |
| Permissions **demandées** + état du consentement | App registrations → l'app → **API permissions** |
| Rôles d'annuaire | Enterprise applications → l'app → **Roles and administrators** |

Le blade `API permissions` ne montre **pas** un app role accordé directement au SP hors manifeste — cas normal en IaC. Seul Enterprise applications → Permissions fait foi. Noter la colonne Type : *Application* vs *Delegated*.

## CLI (fiable)

```powershell
$appId = "<votre-app-id>"
$sp = az rest --method GET --url "https://graph.microsoft.com/v1.0/servicePrincipals(appId='$appId')" | ConvertFrom-Json
# 00000003-0000-0000-c000-000000000000 = appId de Microsoft Graph
$graph = az rest --method GET --url "https://graph.microsoft.com/v1.0/servicePrincipals(appId='00000003-0000-0000-c000-000000000000')" | ConvertFrom-Json
$assignments = az rest --method GET --url "https://graph.microsoft.com/v1.0/servicePrincipals/$($sp.id)/appRoleAssignments" | ConvertFrom-Json

$assignments.value | ForEach-Object {
  $a = $_
  [pscustomobject]@{
    Resource   = $a.resourceDisplayName
    Permission = ($graph.appRoles | Where-Object { $_.id -eq $a.appRoleId }).value
  }
} | Sort-Object Resource, Permission
```

Rôles d'annuaire du même SP :

```powershell
az rest --method GET --url "https://graph.microsoft.com/v1.0/servicePrincipals/$($sp.id)/transitiveMemberOf" `
  --query "value[?'@odata.type'=='#microsoft.graph.directoryRole'].displayName"
```

## Pièges

- La permission de création de groupe s'appelle **`Group.Create`** (singulier), pas `Groups.Create`. `Group.ReadWrite.All` l'englobe.
- `Group.Create` crée le groupe mais ne rend pas le SP propriétaire de ce qu'il vient de créer. Pour « créer *puis* peupler » : positionner explicitement le SP comme owner à la création (l'owner gère ensuite les membres sans permission supplémentaire), sinon il faut `Group.ReadWrite.All` — écriture sur *tous* les groupes du tenant.
- Groupes Microsoft 365 : la *group creation policy* du tenant peut bloquer la création même avec la permission. Ne s'applique pas aux groupes de sécurité créés en app-only.

## Voir aussi

- [[entra-graph-permissions]]
- [[entra-app-registration-vs-enterprise-app]]
- [[azure-cli]]
