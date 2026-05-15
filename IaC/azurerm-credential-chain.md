---
tags: [iac, azure, terraform, authentification]
---

# azurerm — Credential Chain

Le provider et le backend azurerm (OpenTofu / Terraform) parcourent une chaîne de méthodes d'auth dans l'ordre suivant.

## Ordre de priorité

```
1. Service Principal — client secret
      ARM_CLIENT_ID + ARM_CLIENT_SECRET + ARM_TENANT_ID

2. Service Principal — certificat
      ARM_CLIENT_ID + ARM_CLIENT_CERTIFICATE_PATH + ARM_TENANT_ID

3. OIDC (Workload Identity Federation)
      ARM_USE_OIDC=true + ARM_CLIENT_ID + AZURE_FEDERATED_TOKEN_FILE

4. Azure CLI (fallback)
      session active de az login
      lit tenant + subscription depuis le token de session
      appelle : az account get-access-token
```

## Comportement en l'absence de ARM_TENANT_ID

Sans `ARM_TENANT_ID`, le fallback Azure CLI ne passe **aucun flag** à `az account get-access-token` — il utilise le contexte de la session courante (tenant et subscription définis lors du `az login` et de `az account set`).

## Variables d'environnement clés

| Variable | Rôle |
|---|---|
| `ARM_SUBSCRIPTION_ID` | Subscription cible pour le provider |
| `ARM_TENANT_ID` | Tenant (à éviter si Azure CLI auth — voir [[atmos-azure-auth]]) |
| `ARM_CLIENT_ID` | App ID du Service Principal |
| `ARM_CLIENT_SECRET` | Secret du SP (non recommandé en CI) |
| `ARM_USE_AZUREAD` | Équivalent env de `use_azuread_auth = true` dans le backend |
| `AZURE_CONFIG_DIR` | Répertoire du cache MSAL de la CLI Azure |

## Piège : conflit --subscription + --tenant

Le SDK `hashicorp/go-azure-helpers` construit :

```bash
az account get-access-token --subscription $ARM_SUBSCRIPTION_ID --tenant $ARM_TENANT_ID
```

Si les **deux** variables sont définies, la CLI Azure retourne :
```
ERROR: Please specify only one of subscription and tenant, not both
```

**Fix** : ne pas définir `ARM_TENANT_ID` — laisser le tenant venir de la session `az login`.

## Voir aussi

- [[atmos-azure-auth]]
- [[atmos-azure-backend]]
- [[azure-oidc-workload-identity]]
