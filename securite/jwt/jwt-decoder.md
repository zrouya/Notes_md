---
tags: [securite, jwt, snippet, bash, powershell, debug]
---

# JWT — Décoder et inspecter un jeton

Pour comprendre un 401, le premier réflexe est de **lire le jeton** : vérifier `aud`, `iss`, `exp`, et la présence de `roles` ou `scp`.

## Obtenir un jeton (Azure CLI)

```bash
# Jeton pour une API, émis au nom de l'identité connectée à az
az account get-access-token --resource api://mon-api --query accessToken -o tsv
# ou, au format scope :
az account get-access-token --scope api://mon-api/.default --query accessToken -o tsv
```

## Décoder le payload — Bash

```bash
jwt_payload() {
  local p
  p=$(cut -d. -f2 <<< "$1" | tr '_-' '/+')    # Base64URL → Base64
  while (( ${#p} % 4 )); do p+="="; done       # remet le padding
  base64 -d <<< "$p" | jq .
}

jwt_payload "$TOKEN" | jq '{aud, iss, roles, scp, exp: (.exp | todate)}'
```

## Décoder le payload — PowerShell

```powershell
function Get-JwtPayload([string]$Token) {
  $p = $Token.Split('.')[1].Replace('-', '+').Replace('_', '/')
  $p += '=' * ((4 - $p.Length % 4) % 4)
  [Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($p)) | ConvertFrom-Json
}
```

## Détails

- **Outils en ligne** :
  - **jwt.ms** (Microsoft) décode localement et explique les claims Entra ID ;
  - jwt.io est l'équivalent générique.
- Ces outils **décodent** le jeton mais **ne le valident pas** : un jeton bien lisible peut très bien avoir une signature fausse.
- **Attention aux jetons de production** : ne pas coller un jeton encore valide dans un site tiers, car c'est un *bearer token*.
- **Diagnostic d'un 401 avec un jeton Entra ID** :
  - `aud` différent de l'identifiant attendu → mauvais scope demandé, ou confusion entre jetons v1 et v2 ;
  - `roles` absent → le rôle n'a pas été accordé à l'application cliente, ou le consentement admin n'a pas été donné ;
  - `iss` en `sts.windows.net` → jeton v1. Voir [[entra-access-token-versions]].

## Voir aussi

- [[jwt-structure]]
- [[jwt-claims]]
- [[entra-access-token-claims]]
