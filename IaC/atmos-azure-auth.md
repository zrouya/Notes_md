---
tags: [iac, atmos, azure, authentification]
---

# Atmos — Authentification Azure

Atmos gère l'auth Azure via un bloc `auth:` dans `atmos.yaml`, exécuté comme pre-hook avant chaque `terraform plan/apply`.

## Pipe d'authentification

```
az login (CI pipeline)
    │  établit la session CLI + cache MSAL dans $AZURE_CONFIG_DIR/.azure/
    ▼
Atmos pre-hook  (atmos auth terraform pre-hook)
    │  lit la session via le provider azure-cli
    ▼
OpenTofu — azurerm backend + provider
    │  utilise AzureCLICredential (Go Azure SDK)
    │  appelle az account get-access-token en interne
    ▼
Azure (ARM / Blob Storage)
```

## Configuration dans atmos.yaml

```yaml
auth:
  default: azure-dev
  providers:
    azure-cli:
      kind: azure/cli
      spec:
        location: "westeurope"
        # NE PAS mettre tenant_id si ARM_SUBSCRIPTION_ID est aussi défini
        # → provoque az account get-access-token --subscription X --tenant Y (invalide)

  identities:
    azure-dev:
      default: true
      kind: azure/subscription
      via:
        provider: azure-cli
      principal:
        subscription_id: !env ARM_SUBSCRIPTION_ID
```

## Provider kinds disponibles

| Kind | Usage |
|---|---|
| `azure/cli` | Dev / CI avec `az login` |
| `azure/oidc` | CI sans secret (Workload Identity Federation) |
| `azure/device-code` | Auth interactive navigateur |

## Pièges connus

### Conflit `--subscription` + `--tenant`

Si `ARM_TENANT_ID` **et** `ARM_SUBSCRIPTION_ID` sont tous deux définis en env, le Go Azure SDK construit :

```bash
az account get-access-token --subscription X --tenant Y
# → ERROR: Please specify only one of subscription and tenant, not both
```

**Fix** : ne pas exposer `ARM_TENANT_ID` comme variable d'environnement dans le pipeline.  
Le tenant est déjà implicite dans la session `az login --tenant $tenantId`.

### Cache MSAL instable en CI

`az login` écrit le cache dans `$HOME/.azure/`. Si `$HOME` diffère entre steps du pipeline (runners Docker), le token est introuvable.

**Fix** : fixer `AZURE_CONFIG_DIR` à un chemin stable **avant** `az login`, identique pour tout le job.

```bash
export AZURE_CONFIG_DIR=/tmp/azure-cli-cache
az login --service-principal -u "$client_id" -p "$client_secret" --tenant "$tenant_id"
```

## Voir aussi

- [[atmos-azure-backend]]
- [[azure-oidc-workload-identity]]
- [[atmos-overview]]
