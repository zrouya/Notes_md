---
tags: [mcp, agents, architecture, llm]
---

# MCP — Rôles host / client / server

Le Model Context Protocol distingue trois rôles. Le piège de vocabulaire : « client » n'est pas un programme, c'est un **objet** instancié dans le processus du host.

## Les trois rôles

| Rôle | Qui c'est | Responsabilité |
|---|---|---|
| **Host** | L'application lancée par l'utilisateur (Claude Code, Claude Desktop, ton app) | Possède le modèle et l'utilisateur |
| **Client** | Un connecteur *par serveur*, instancié par le host | Une session JSON-RPC avec un serveur |
| **Server** | Le processus ou service qui expose les capacités | Fournit `tools`, `resources`, `prompts` |

## Règle structurante : 1 client = 1 serveur

Un host qui parle à 5 serveurs instancie 5 clients. C'est ce cloisonnement qui garantit qu'un serveur ne voit jamais le contexte d'un autre.

## Ce que le host possède en exclusivité

- **Le modèle** : clé d'API, boucle d'inférence, fenêtre de contexte
- **L'utilisateur** : interface, confirmations, politique de permissions

Toute l'architecture en découle : quand un serveur utilise la capacité `sampling` (« fais-moi une inférence »), c'est le host qui l'exécute — c'est lui qui a la clé. Idem pour `elicitation` (« demande une valeur à l'utilisateur »). **Un serveur ne parle jamais directement ni au modèle ni à l'humain.**

## Responsabilités du host

- Lire la configuration → liste des serveurs à joindre
- Instancier et détruire les clients
- Agréger les catalogues `tools/list` de N serveurs en une seule liste pour le modèle
- Router les `tool_use` du modèle vers le bon client
- **Appliquer les autorisations** — la spec MCP ne dit rien là-dessus, c'est entièrement au host

## Voir aussi

- [[mcp-instanciation-client]]
- [[mcp-transports]]
- [[mcp-handshake-initialize]]
- [[mcp-options-architecture]]
