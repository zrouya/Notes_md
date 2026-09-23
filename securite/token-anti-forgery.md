---
tags: [securite, csrf, token]
---

# Token anti forgery

Les tokens d'antiforgery constituent une réponse aux [[attaques-csrf-xsrf|attaques de type XSRF ou CSRF]] au sein d'une application web. Ils permettent de sécuriser notamment les requêtes POST exposées par le serveur.

## Fonctionnement

1. **Génération de token** : lorsqu'un formulaire est généré à partir d'une vue, deux tokens antiforgery sont créés.
2. **Transmission des tokens** : les 2 tokens sont transmis au client par les moyens suivants :
   - un token est généralement envoyé dans le corps du formulaire lui-même, sous forme d'un champ caché ;
   - le second token est envoyé avec les headers de la requête (qui a permis de récupérer le formulaire), et est stocké côté client dans un cookie.
3. **Validation des tokens** : lors de la réception d'une requête POST (soumission du formulaire), le serveur extrait le token du champ caché et le compare au token du cookie. Les deux ne sont pas strictement identiques, mais le serveur peut les utiliser ensemble pour valider la demande.

## Ce que ce dispositif garantit

- Que la requête vient d'un **formulaire légitimement généré** par l'application, via le token dans le formulaire.
- Que la requête vient du **bon utilisateur**, via le token dans le cookie. Les scripts malveillants sur des sites tiers ne peuvent pas lire les cookies d'un autre domaine (grâce à [[same-origin-policy-sop|la politique de même origine]]), donc ils ne peuvent pas copier le token du cookie pour le joindre à leur requête forgée.

## Voir aussi

- [[attaques-csrf-xsrf]]
- [[same-origin-policy-sop]]
