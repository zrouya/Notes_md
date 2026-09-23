

Les tokens d'antiforgery constituent une réponse aux [[Attaques CSRF (XSRF)|attaques de type XSRF ou CSRF]], au sein d'une application web. Ils permettent de sécuriser notamment les requêtes POST exposées par le serveur.

1. **Génération de token** :
	
	- Lorsqu'un formulaire est généré à partir d'une vue, deux tokens antiforgery sont créés.

3. **Transmission des tokens** :
	
	Les 2 tokens sont transmis au client par les moyens suivant : 
    - Un token est généralement envoyé dans le corps du formulaire lui-même, généralement sous forme d'un champ caché.
    - Le second token est envoyé avec les headers de la requête (qui a permis de récupérer le formulaire), et est stocké côté client dans un cookie.
    
3. **Validation des tokens** :
    
    - Lors de la réception d'une requête POST (lorsque le client soumet le formulaire), le serveur va extraire le token du champ caché du formulaire et le comparer au token du cookie. Les deux ne sont pas strictement identiques, mais le serveur peut les utiliser ensemble pour valider la demande. 


Ce dispositif permet : 

- De garantir que la requête vient d'un **formulaire légitimement généré** par l'application, via le token dans le formulaire 
- De garantir que la requête vient du **bon utilisateur**, via le token dans le cookie. Les scripts malveillants sur des sites tiers ne peuvent pas lire les cookies d'un autre domaine (grâce à [[Same Origin Policy (SOP)|la politique de même origine]]), donc ils ne peuvent pas copier le token du cookie pour le joindre à leur requête forgée.