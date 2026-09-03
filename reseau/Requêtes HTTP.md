---
tags: [reseau, http, api]
---

# Requêtes HTTP

Structure d'un échange HTTP. Toute la complexité tient dans les en-têtes ; la structure tient en cinq lignes.

## Requête

```http
GET /v1/users?page=2 HTTP/1.1
Host: api.exemple.com          <- obligatoire : qui je veux joindre
Accept: application/json
Authorization: Bearer eyJhbGci...
                               <- ligne vide = fin des en-tetes
```

L'en-tête `Host` rend l'hébergement mutualisé possible : la connexion TCP mène à une IP, mais `Host` désigne le site voulu parmi ceux qu'elle sert.

## Réponse

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1842
Cache-Control: max-age=60, private

{"users":[...]}
```

## Classes de statut

| Classe | Sens | Courants |
|--------|------|----------|
| `2xx` | Succès | 200, 201 Created, 204 No Content |
| `3xx` | Redirection | 301 permanente, 302 temporaire, 304 Not Modified |
| `4xx` | **Le client a tort** — rejouer donnera le même résultat | 400, 401, 403, 404, 429 |
| `5xx` | **Le serveur a tort** — réessayer a du sens | 500, 502, 503, 504 |

## Les distinctions qui reviennent

- **401** = « je ne sais pas qui tu es » · **403** = « je sais, et tu n'as pas le droit »
- **502** = un intermédiaire a reçu une réponse invalide de l'amont
- **504** = l'amont **n'a pas répondu du tout**

502 vs 504 oriente directement le diagnostic vers le proxy ou vers le backend.

## Méthodes

| Méthode | Idempotente | Sûre |
|---------|-------------|------|
| `GET`, `HEAD` | oui | oui |
| `PUT`, `DELETE` | oui | non |
| `POST`, `PATCH` | **non** | non |

*Idempotente* = rejouer N fois produit le même état. C'est ce qui autorise un retry automatique.

## Voir aussi

- [[HTTP(S)]] · [[http-versions]]
- [[erreurs-connexion-econnrefused]]
