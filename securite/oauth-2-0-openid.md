---
tags: [securite, oauth, openid, authentification]
---

# OAuth 2.0 / OpenID Connect

OAuth 2.0 est un protocole d'autorisation qui permet à une application d'obtenir un accès limité aux ressources d'un compte utilisateur sur un autre service. OpenID Connect en est une extension qui ajoute l'authentification de l'utilisateur.

## OAuth 2.0

Exemple : une application cliente veut accéder à la liste des contacts Google de l'utilisateur.

- L'utilisateur **s'authentifie directement** auprès du **fournisseur d'authentification** (ex : Google).
- Le fournisseur délivre un **code d'autorisation** à l'application.
- L'application utilise ce code pour **obtenir un jeton d'accès**, qui lui permet d'accéder aux ressources de l'utilisateur.
- Outre le jeton d'accès, le fournisseur renvoie un **jeton de rafraichissement**, qui permet à l'application de renouveler ultérieurement son jeton d'accès (et son jeton de rafraichissement).

Workflow dans le cas d'Azure :

![[Oauth.svg]]

## OpenID Connect

OpenID Connect est une extension de OAuth 2.0 qui permet l'authentification de l'utilisateur en plus de l'autorisation. Une application délègue l'authentification à un fournisseur OpenID, qui renvoie un jeton d'identité après l'authentification de l'utilisateur.

## Sécurité dans OAuth 2.0 et OpenID Connect

- **Confidentialité des communications** : les communications sont sécurisées via HTTPS pour éviter l'interception ou la modification des données en transit.
- **Enregistrement préalable du client** : les applications tierces doivent s'enregistrer auprès du fournisseur d'authentification avant de pouvoir utiliser le service. Lors de l'enregistrement, l'application reçoit un **identifiant client et un secret client**.
- **Redirections sécurisées** : le fournisseur utilise une **URL de redirection pré-enregistrée** pour renvoyer l'utilisateur à l'application.
- **Protection contre les attaques de type CSRF** : l'application envoie une valeur "state" unique lors de la demande d'autorisation, que le fournisseur renvoie avec le code d'autorisation.
- **Protection des jetons** : les jetons d'accès, codes d'autorisation et jetons d'identité sont conçus pour être difficiles à deviner et à falsifier, et leur utilisation est limitée dans le temps.

## Voir aussi

- [[authentification]]
- [[token-anti-forgery]]
