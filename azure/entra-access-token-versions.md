---
tags: [azure, entra, jwt, token, troubleshooting]
---

# Entra ID — Jetons v1 vs v2

Entra ID émet deux formats d'access token, qui diffèrent par leur **issuer** et leur **audience**. Mal configurer la version attendue est l'une des causes les plus fréquentes de 401.

## Comparaison

| | v1.0 | v2.0 |
|---|---|---|
| `iss` | `https://sts.windows.net/<tid>/` | `https://login.microsoftonline.com/<tid>/v2.0` |
| `aud` | La valeur demandée, souvent l'**App ID URI** (`api://mon-api`) | Toujours le **client ID** (GUID) de l'API |
| Application cliente | `appid` | `azp` |
| Metadata URL | `.../<tid>/.well-known/openid-configuration` | `.../<tid>/v2.0/.well-known/openid-configuration` |

## Qui décide de la version ?

**C'est l'API (la ressource), et non le client.**

- Le point de terminaison utilisé par le client (`/oauth2/token` ou `/oauth2/v2.0/token`) **ne change rien** au format de l'access token.
- La version est déterminée par le manifeste de l'App Registration de l'**API** :

```json
"accessTokenAcceptedVersion": 2      // null ou 1 → jetons v1 ; 2 → jetons v2
```

Dans le portail : *App Registration de l'API → Manifest*. Dans Microsoft Graph, la propriété correspondante est `api.requestedAccessTokenVersion`.

## Détails

- **Symptôme typique** : la validation utilise la metadata v2, donc attend un `iss` v2, mais l'API émet des jetons v1 (`sts.windows.net`). Le jeton est rejeté pour cause d'issuer invalide.
- **Deux façons de corriger** :
  1. passer `accessTokenAcceptedVersion` à `2` (recommandé pour une nouvelle API) ;
  2. ou accepter explicitement les deux émetteurs et les deux audiences dans le validateur.
- **Audience** : si l'API reçoit des jetons des deux versions, il faut accepter **l'App ID URI et le client ID**.
- **Jetons Microsoft Graph** : ils restent en v1, avec `aud` = `https://graph.microsoft.com` ou `00000003-0000-0000-c000-000000000000`.

## Voir aussi

- [[jwt-audience-issuer]]
- [[entra-access-token-claims]]
- [[entra-app-registration-vs-enterprise-app]]
- [[apim-validate-jwt]]
