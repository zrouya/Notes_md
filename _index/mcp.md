---
tags: [index, mcp]
---

# MCP — Map of Content

Map of Content pour le Model Context Protocol et l'architecture des clients agentiques.

## Concepts fondamentaux

- [[mcp-roles-host-client-server]] — Les trois rôles, la règle 1 client = 1 serveur, ce que le host possède en exclusivité
- [[mcp-transports]] — stdio vs Streamable HTTP, le piège de `stdout`, HTTP+SSE déprécié
- [[mcp-handshake-initialize]] — Séquence `initialize` / `initialized`, négociation de version et de capacités, `listChanged`
- [[mcp-instanciation-client]] — Quand et comment un client est instancié, préfixage des outils, immédiat vs paresseux

## Sécurité

- [[mcp-authentification]] — Variables d'env en stdio, OAuth 2.1 en HTTP, confused deputy et DNS rebinding
- [[mcp-conception-outils]] — search/fetch en deux temps, plafonds dans l'outil, read-only par construction, injection de prompt

## Mise en œuvre

- [[mcp-options-architecture]] — Les quatre options (Claude Code / Agent SDK / client maison / MCP connector) et le critère de choix
- [[mcp-configuration-claude-code]] — `claude mcp add`, `.mcp.json`, les trois portées, ce qui manque au-delà de la config
- [[mcp-connector-api-claude]] — Le connector de l'API : `mcp_servers` + `mcp_toolset`, HTTP uniquement
