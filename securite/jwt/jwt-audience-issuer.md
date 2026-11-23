---
tags: [securite, jwt, oauth, audience, issuer]
---

# JWT — Audience (`aud`) et Issuer (`iss`)

**L'audience indique à qui le jeton est destiné, c'est-à-dire quelle API a le droit de l'accepter.** L'issuer indique qui l'a fabriqué. Une API doit vérifier **les deux** : « le jeton vient-il d'un émetteur de confiance ? » et « a-t-il été fait pour moi ? ».

## L'analogie du chèque

| Chèque | JWT |
|---|---|
| Signature de la banque : le chèque est authentique | Signature + `iss` |
| « À l'ordre de Mme Durand » : seule elle peut l'encaisser | **`aud`** |
| Date de validité | `exp` / `nbf` |

Un guichetier qui ne vérifie que la signature de la banque paie n'importe quel chèque authentique à n'importe qui. Vérifier l'audience, c'est se demander **« ce chèque est-il à mon ordre ? »**.

## Les 4 acteurs d'un jeton

| Claim | Question | Exemple (appel service à service) |
|---|---|---|
| `iss` | **Qui a fabriqué** le jeton ? | Entra ID (le tenant) |
| `azp` / `appid` | **Qui l'a demandé** et l'envoie ? | L'application cliente |
| `sub` / `oid` | **Au sujet de qui** est-il ? | L'application cliente, ou l'utilisateur en délégué |
| `aud` | **Pour qui** est-il fabriqué ? | **L'API appelée** |

**Confusion classique** : l'audience n'est **pas l'appelant**, c'est le **destinataire**. L'appelant est `azp` / `appid`.

## Comment `aud` arrive dans le jeton

1. Le client demande au fournisseur d'identité un jeton **pour** une ressource : `scope=api://mon-api/.default`.
2. Le fournisseur d'identité inscrit `aud = <identifiant de mon-api>` dans le jeton, puis le **signe**.
3. Le client ne peut plus modifier `aud` : toute modification casserait la signature.

L'audience est donc **choisie par le client** au moment de la demande, puis **garantie par l'émetteur**.

Chez Entra ID :
- jeton **v2** : `aud` = client ID (GUID) de l'API ;
- jeton **v1** : `aud` = la valeur demandée, souvent l'App ID URI (`api://...`).

Voir [[entra-access-token-versions]].

## Issuer (`iss`) — qui a émis le jeton

- C'est l'URL de l'autorité qui a émis le jeton. Chez Entra ID, elle contient l'identifiant du tenant : `https://login.microsoftonline.com/<tenant>/v2.0`.
- Le contrôler revient à n'accepter **que les jetons de son propre fournisseur d'identité ou de son propre tenant**.

## Pourquoi la signature ne suffit pas

**Un même émetteur signe les jetons de toutes les API, avec les mêmes clés.** Chez Entra ID, ces clés sont même **communes à tous les tenants**. Une signature valide prouve seulement « Entra ID a émis ce jeton » :

- elle ne prouve **pas** que le jeton vient de mon tenant : c'est `iss` (ou `tid`) qui le prouve ;
- elle ne prouve **pas** que le jeton a été fait pour moi : c'est `aud` qui le prouve.

Les attaques possibles sans ces contrôles sont détaillées dans [[jwt-audience-attaques]].

## Détails

- `aud` peut être une liste. Le jeton est accepté si **l'une** des valeurs correspond.
- Une API qui reçoit des jetons v1 et v2 accepte souvent **l'App ID URI et le client ID**.
- Un *ID token* a pour audience **le client** lui-même : le contrôle de `aud` le rejette automatiquement. Voir [[access-token-vs-id-token]].
- `aud` **n'autorise pas** l'appelant : il garantit seulement que le jeton est destiné à l'API. L'autorisation repose sur `roles` / `scp`. Voir [[authentification-vs-autorisation]].

## Voir aussi

- [[jwt-audience-attaques]]
- [[jwt-claims]]
- [[jwt-validation-checklist]]
- [[entra-access-token-versions]]
