---
tags: [iac, atmos, azure, authentification]
---

# Atmos — Authentification Azure

L'auth Azure dans un pipeline Atmos repose sur la session `az login` et la credential chain du provider azurerm — sans bloc `auth:` dans `atmos.yaml`.

## Pipe d'authentification (solution retenue)

```
az login --service-principal --tenant $tenantId
    │  établit la session CLI + cache MSAL dans $AZURE_CONFIG_DIR
    ▼
az account set --subscription $subId
    │  définit la subscription active dans la session
    ▼
atmos terraform plan <component> -s <stack>
    │  (pas de pre-hook — bloc auth: supprimé)
    ▼
OpenTofu — azurerm backend + provider
    │  credential chain : lit ARM_SUBSCRIPTION_ID puis session CLI
    │  appelle az account get-access-token (sans flags explicites)
    ▼
Azure (ARM / Blob Storage)
```

## Variables d'environnement requises dans le pipeline

```bash
export AZURE_CONFIG_DIR=/tmp/azure-cli-cache   # chemin stable pour le cache MSAL
export ARM_SUBSCRIPTION_ID="$devSubscriptionId" # subscription pour le provider azurerm

# NE PAS exporter ARM_TENANT_ID
# → le tenant est porté par la session az login, pas par une variable
```

## Pourquoi ne pas utiliser le bloc auth: d'Atmos

Le bloc `auth:` déclenche un pre-hook (`atmos auth terraform pre-hook`) qui appelle en interne :

```bash
az account get-access-token --subscription $ARM_SUBSCRIPTION_ID --tenant $ARM_TENANT_ID
```

La CLI Azure **refuse** cette combinaison de flags — c'est une limitation de `az account get-access-token`. Il faut l'un ou l'autre, pas les deux.

Source : `hashicorp/go-azure-helpers` — `azure_cli_token.go` construit systématiquement `--subscription` depuis `ARM_SUBSCRIPTION_ID` et ajoute `--tenant` si `ARM_TENANT_ID` est défini.

## Piège : cache MSAL instable en CI

`az login` écrit le cache dans `$HOME/.azure/`. Si `$HOME` diffère entre steps du pipeline (runners Docker), le token est introuvable.

**Fix** : définir `AZURE_CONFIG_DIR` à un chemin stable avant `az login`.

```bash
export AZURE_CONFIG_DIR=/tmp/azure-cli-cache
az login --service-principal -u "$client_id" -p "$client_secret" --tenant "$tenant_id"
```

## Provider kinds (pour référence, si auth: réactivé)

| Kind | Usage |
|---|---|
| `azure/cli` | Dev / CI avec `az login` |
| `azure/oidc` | CI sans secret (Workload Identity Federation) |
| `azure/device-code` | Auth interactive navigateur |

## Voir aussi

- [[atmos-azure-backend]]
- [[azurerm-credential-chain]]
- [[azure-oidc-workload-identity]]
- [[atmos-overview]]
