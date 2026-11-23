---
tags: [securite, jwt, oauth, audience, issuer]
---

# JWT — Audience (`aud`) et Issuer (`iss`)

Ce sont les deux claims qui répondent aux questions de **confiance** : « est-ce que je fais confiance à celui qui a émis ce jeton ? » (`iss`) et « est-ce que ce jeton m'est bien destiné ? » (`aud`). Une API doit vérifier **les deux**.

## Issuer (`iss`) — qui a émis le jeton

- C'est l'URL de l'autorité qui a émis le jeton. Chez Entra ID, elle contient l'identifiant du tenant : `https://login.microsoftonline.com/<tenant>/v2.0`.
- Vérifier `iss` revient à n'accepter **que les jetons de son propre fournisseur d'identité ou de son propre tenant**.
- **Piège multi-tenant** : la signature est faite par des clés **communes à tous les tenants** Entra ID. Une signature valide ne prouve donc pas que le jeton vient de **votre** tenant : c'est le contrôle de `iss` (ou de `tid`) qui le prouve.

## Audience (`aud`) — à qui le jeton est destiné

Un jeton est émis **pour une ressource précise** (une API). L'audience est l'identifiant de cette ressource.

Exemple chez Entra ID :

```text
Le client demande  : scope=api://mon-api/.default
Il reçoit un jeton : "aud": "api://mon-api"   (jeton v1)
                ou : "aud": "<client-id-de-mon-api>"   (jeton v2)
```

Le client **ne choisit pas `aud` directement** : il demande un *scope* et le fournisseur d'identité en déduit l'audience.

## Pourquoi l'audience est indispensable : le *confused deputy*

Le scénario, si l'API A **ne vérifie pas** l'audience :

1. Un utilisateur appelle l'API B avec un jeton destiné à B.
2. L'API B, malveillante ou compromise, réutilise ce jeton pour appeler l'API A.
3. L'API A accepte le jeton, car la signature et l'émetteur sont valides.

Résultat : B agit sur A au nom de l'utilisateur, sans y avoir été autorisée.

La règle : **une API n'accepte que les jetons dont `aud` est son propre identifiant**. Pour appeler A, B doit obtenir un nouveau jeton destiné à A, par exemple via le flux *On-Behalf-Of*.

## Détails

- `aud` peut être une liste. Le jeton est accepté si **l'une** des valeurs correspond.
- Il est courant d'accepter à la fois l'*App ID URI* et le client ID de l'API, pour couvrir les jetons v1 et v2. Voir [[entra-access-token-versions]].
- Un *ID token* a pour audience **le client** lui-même. Contrôler l'audience permet donc aussi de rejeter un ID token présenté à une API. Voir [[access-token-vs-id-token]].

## Voir aussi

- [[jwt-claims]]
- [[jwt-validation-checklist]]
- [[entra-access-token-versions]]
