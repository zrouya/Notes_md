
## OAuth 2.0

OAuth 2.0 est un protocole d'autorisation qui permet à une application d'obtenir un accès limité aux ressources d’un compte utilisateur sur un autre service.

Exemple : Une application (appelée application cliente) veut accéder à la liste des contacts Google de l’utilisateur.

- L'utilisateur **s'authentifie directement** auprès du **fournisseur d'authentification** (ex: Google),
- Le fournisseur délivre un **code d'autorisation** à l'application.
- L'application utilise ensuite ce code pour **obtenir un jeton d'accès**, qui lui permet d'accéder aux ressources de l'utilisateur.
- Outre le jeton d’accès, le fournisseur renvoie un **jeton de rafraichissement**, qui permet à l’application de **renouveler ultérieurement son jeton d’accès** (et également son jeton de rafraichissement).

Workflow dans le cas d'Azure :

![[Oauth.svg]]

## OpenID Connect

OpenID Connect est une extension de OAuth 2.0 qui permet l'authentification de l'utilisateur en plus de l'autorisation. Il permet à une application de déléguer l'authentification à un fournisseur OpenID, qui renvoie un jeton d'identité à l'application après l'authentification de l'utilisateur.

## Sécurité dans OAuth 2.0 et OpenID Connect

- **Confidentialité des communications:** Les communications sont **sécurisées en utilisant HTTPS** pour éviter l'interception ou la modification des données en transit.
- **Enregistrement préalable du client** : Les applications tierces (les "clients") doivent s'enregistrer auprès du fournisseur d'authentification (le "serveur d'autorisation" dans le cas d'OAuth 2.0, ou le "fournisseur OpenID" dans le cas d'OpenID Connect) avant de pouvoir utiliser le service. Lors de l'enregistrement, l'application reçoit un **identifiant client et un secret client**, qu'elle doit utiliser pour s'authentifier auprès du fournisseur d'authentification.
- **Redirections sécurisées:** Le fournisseur d'authentification utilise une **URL de redirection pré-enregistrée** (lors de l’enregistrement du client) pour renvoyer l'utilisateur à l'application.
- **Protection contre les attaques de type CSRF:** L'application envoie une valeur "state" unique lors de la demande d'autorisation, et le fournisseur d'authentification la renvoie avec le code d'autorisation.
- **Protection des jetons:** Les jetons d'accès, les codes d'autorisation, et les jetons d'identité sont conçus pour être difficiles à deviner et à falsifier, et leur utilisation est limitée dans le temps.