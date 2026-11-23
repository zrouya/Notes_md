---
tags: [securite, oauth, openid, jwt, token]
---

# Access token vs ID token vs Refresh token

OAuth 2.0 et OpenID Connect manipulent trois sortes de jetons. Chacun a **un seul destinataire légitime** : il ne faut pas les utiliser l'un à la place de l'autre.

## Comparaison

| | Access token | ID token | Refresh token |
|---|---|---|---|
| Défini par | OAuth 2.0 | OpenID Connect | OAuth 2.0 |
| Sert à | **Appeler une API** | **Prouver au client** qui s'est connecté | **Obtenir de nouveaux jetons** |
| Destinataire (`aud`) | L'API (ressource) | Le client lui-même | Le serveur d'autorisation uniquement |
| Format | Souvent un JWT, mais **opaque pour le client** | Toujours un JWT | Opaque |
| Durée de vie typique | Courte (~1 h chez Entra ID) | Courte | Longue (jours ou semaines), avec rotation |
| Transmis à | L'API, dans `Authorization: Bearer` | Personne : il reste chez le client | Le point de terminaison `/token` |

## Détails

- **Un client ne doit pas lire son access token**. Seule l'API à laquelle il est destiné doit l'interpréter, car son format peut changer à tout moment, voire être chiffré.
- **Une API ne doit jamais accepter un ID token** : son `aud` est le client et non l'API. Le contrôle de l'audience suffit à le rejeter. Voir [[jwt-audience-issuer]].
- **Rôle du refresh token** : c'est lui qui permet de garder des access tokens à durée de vie courte sans redemander à l'utilisateur de se connecter.
  - Il est précieux, donc à stocker côté serveur ou dans un stockage sécurisé.
  - Il est **révocable** côté fournisseur d'identité (au contraire des JWT).
- **Pas de refresh token en *client credentials*** : l'application dispose déjà de son secret ou de son certificat, elle redemande simplement un jeton quand il expire. Voir [[oauth-grant-types]].
- **Révocation** : un access token JWT reste valide jusqu'à `exp`, même après une désactivation du compte. D'où les durées courtes.
  - Entra ID propose la **Continuous Access Evaluation (CAE)**, qui permet d'interrompre plus tôt une session sur les ressources compatibles.

## Voir aussi

- [[oauth-2-0-openid]]
- [[oauth-grant-types]]
- [[jwt-pieges-securite]]
