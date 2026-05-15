---
tags: [azure, oidc, sécurité, gitlab-ci, atmos]
---

# Azure Workload Identity Federation (OIDC)

Authentification sans secret : le CI génère un JWT éphémère (~5 min) qu'Azure valide via une Federated Identity Credential configurée sur le Service Principal.

## Flux

```
GitLab CI Job
  │  génère JWT OIDC éphémère via id_tokens
  ▼
Azure Entra ID
  │  valide issuer + subject contre la Federated Identity Credential
  ▼
Atmos (kind: azure/oidc)
  │  échange le JWT contre un access token Azure
  ▼
OpenTofu / azurerm backend
```

## Configuration Azure — Federated Identity Credential

Sur l'App Registration du Service Principal, ajouter :

| Champ | Valeur |
|---|---|
| Issuer | `https://gitlab.com` (ou URL instance GitLab) |
| Subject | `project_path:<groupe>/<projet>:ref_type:branch:ref:main` |
| Audience | `api://AzureADTokenExchange` |

Le `subject` peut filtrer par branche ou environment GitLab — un seul SP peut couvrir dev/rec/prod avec des credentials séparés.

## Configuration Atmos (atmos.yaml)

```yaml
auth:
  providers:
    azure-oidc:
      kind: azure/oidc
      spec:
        tenant_id: !env ARM_TENANT_ID
        client_id: !env ARM_CLIENT_ID   # App ID du SP — plus de client_secret

  identities:
    azure-dev:
      kind: azure/subscription
      via:
        provider: azure-oidc
      principal:
        subscription_id: !env ARM_SUBSCRIPTION_ID
```

## Pipeline GitLab

```yaml
job:
  id_tokens:
    AZURE_OIDC_TOKEN:
      aud: api://AzureADTokenExchange

  variables:
    ARM_TENANT_ID: $TENANT_ID
    ARM_SUBSCRIPTION_ID: $DEV_SUBSCRIPTION_ID
    ARM_CLIENT_ID: $DEV_SP_CLIENT_ID
    AZURE_FEDERATED_TOKEN_FILE: /tmp/azure-oidc-token

  script:
    - echo $AZURE_OIDC_TOKEN > /tmp/azure-oidc-token
    - cd "${IAC_DIR}"
    - atmos terraform plan "${COMPONENT}" -s "${STACK}"
    # Plus d'az login — Atmos gère l'échange du token
```

## Comparaison avec Service Principal + secret

| Critère | SP + secret | OIDC |
|---|---|---|
| Secrets à gérer | `client_secret` à stocker en CI var | Aucun |
| Rotation | Manuelle, risque d'expiration | N/A |
| Durée de vie token | ~1h (access token) | ~5 min (JWT) |
| Risque de fuite | Oui | Non |
| Conflit subscription/tenant | Oui (voir [[atmos-azure-auth]]) | Non |

## Prérequis

- GitLab ≥ 15.7 (feature `id_tokens`)
- Atmos ≥ 1.198.0 (support `kind: azure/oidc`)
- SP avec rôles ARM + `Storage Blob Data Contributor` sur le storage account

## Voir aussi

- [[atmos-azure-auth]]
- [[atmos-azure-backend]]
- [[Azure Entra (anciennement Azure Active Directory)]]
