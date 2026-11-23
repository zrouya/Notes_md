---
tags: [securite, jwt, oauth, audience, vulnerabilites]
---

# JWT — Ce que l'audience protège (et ce qu'elle ne protège pas)

Trois situations concrètes pour comprendre le rôle de `aud` : deux attaques qu'il bloque, et une limite qu'il faut compléter par un contrôle d'autorisation.

## Scénario 1 — Rejeu par une API intermédiaire (*confused deputy*)

```text
Utilisateur ──jeton(aud=Météo)──▶ API Météo (compromise)
                                     │ réutilise le même jeton
                                     ▼
                                  API Paie ── vérifie aud ? ──┬─ non → accepte  ✗
                                                              └─ oui → 401      ✓
```

- La signature et l'émetteur sont valides : seule l'audience trahit le détournement.
- **Parade** : l'API Paie n'accepte que `aud = Paie`.
- **La bonne pratique pour un appel chaîné** : si Météo a un besoin légitime d'appeler Paie au nom de l'utilisateur, elle obtient un **nouveau jeton** destiné à Paie, via le flux On-Behalf-Of. Voir [[oauth-grant-types]].

## Scénario 2 — Le rôle qui change de sens

Les `roles` et `scp` sont **définis par chaque API**. Leur valeur n'a de sens que pour cette API.

```json
{ "aud": "api://meteo", "roles": ["Admin"] }   // = administrateur de Météo, rien d'autre
```

- Si l'API Paie vérifie seulement `roles contient "Admin"` sans regarder `aud`, **un administrateur de Météo devient administrateur de Paie**.
- Le risque est réel : des noms de rôles génériques (`Admin`, `Read`, `Api.Call`) se retrouvent d'une API à l'autre.

> **`roles` et `scp` n'ont de sens que par rapport à `aud`.** Vérifier un rôle sans vérifier l'audience revient à regarder un badge sans vérifier de quel bâtiment il ouvre les portes.

## Scénario 3 — La limite : `aud` n'est pas une autorisation

Chez Entra ID, **n'importe quelle application du tenant peut demander un jeton dont l'audience est votre API**. Elle recevra un jeton :

- valide (bonne signature, bon émetteur) ;
- avec la **bonne audience** ;
- mais **sans rôle**.

Si l'API ne vérifie que `aud`, toutes les applications du tenant peuvent l'appeler.

**Parades** :
- exiger un rôle ou un scope (`required-claims` dans APIM) ;
- et/ou activer *Assignment required* sur l'Enterprise Application de l'API, pour qu'Entra ID refuse dès l'émission de délivrer un jeton aux applications sans rôle.

## Ce que chaque contrôle garantit

| Contrôle | Garantie |
|---|---|
| Signature | Le jeton n'a pas été falsifié |
| `iss` | Il vient de **mon** fournisseur d'identité, de **mon** tenant |
| `exp` | Il est encore valide |
| `aud` | Il a été **fabriqué pour moi** |
| `roles` / `scp` | **Cet appelant** a le droit de m'appeler |

Retirer une seule ligne de ce tableau ouvre l'une des attaques ci-dessus.

## Voir aussi

- [[jwt-audience-issuer]]
- [[authentification-vs-autorisation]]
- [[jwt-validation-checklist]]
- [[entra-scopes-vs-app-roles]]
- [[jwt-pieges-securite]]
