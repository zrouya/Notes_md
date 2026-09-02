---
tags: [mcp, authentification, oauth, securite]
---

# MCP — Authentification

Le modèle d'authentification dépend entièrement du transport.

## En stdio : pas d'auth réseau

La confiance vient du fait que le host contrôle le lancement du sous-processus. Les secrets passent par les **variables d'environnement** du processus enfant.

```json
{
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-xyz"],
  "env": { "API_TOKEN": "..." }
}
```

## En HTTP : OAuth 2.1

Le serveur MCP joue le rôle de *resource server*.

1. Appel sans jeton → `401` + entête `WWW-Authenticate` pointant vers les métadonnées de ressource protégée (RFC 9728)
2. Découverte de l'authorization server puis de ses métadonnées (RFC 8414)
3. Enregistrement dynamique du client (RFC 7591) s'il n'a pas de `client_id`
4. Authorization code flow avec **PKCE obligatoire** et un paramètre `resource` (RFC 8707)
5. Le jeton part en `Authorization: Bearer <token>` sur chaque requête

Le paramètre `resource` est le point important : il **lie le jeton à ce serveur MCP précis**, ce qui empêche sa réutilisation ailleurs.

## Le raccourci pragmatique

Beaucoup de serveurs internes se contentent d'un bearer statique ou d'une clé d'API en entête. Acceptable en contexte privé, mais hors spec et ne monte pas en charge sur du multi-utilisateur.

## Deux pièges côté serveur

- **Confused deputy** : valider que le jeton reçu a bien été *émis pour toi*, pas seulement qu'il est valide
- **DNS rebinding** : valider l'entête `Origin` sur les serveurs HTTP locaux, sinon une page web peut piloter ton serveur

## Voir aussi

- [[mcp-transports]]
- [[OAuth 2.0 - OpenID]]
- [[mcp-conception-outils]]
