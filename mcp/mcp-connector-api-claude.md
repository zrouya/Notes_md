---
tags: [mcp, api-claude, connector, beta]
---

# MCP connector de l'API Claude

Le seul cas (avec Managed Agents) où **Anthropic joue le rôle de client MCP**. Tu déclares le serveur dans ta requête `/v1/messages` et les serveurs d'Anthropic s'y connectent.

## Les deux moitiés obligatoires

```python
client.beta.messages.create(
    model="claude-opus-5",
    max_tokens=16000,
    betas=["mcp-client-2025-11-20"],
    mcp_servers=[{"type": "url", "url": "https://exemple.com/mcp", "name": "mon-api"}],
    tools=[{"type": "mcp_toolset", "mcp_server_name": "mon-api"}],
    messages=[...],
)
```

**Déclarer `mcp_servers` seul est rejeté en erreur de validation.** Il faut aussi le `mcp_toolset` correspondant, avec le même `name`.

## Contraintes

- **stdio impossible** — personne chez Anthropic ne lance un processus sur ta machine. HTTP distant uniquement.
- Le serveur doit être **joignable depuis Internet**. Éliminatoire pour des sources on-prem.
- Beta : entête / flag `mcp-client-2025-11-20`

## Quand c'est le bon choix

Serveur MCP SaaS ou exposé publiquement, et tu ne veux gérer aucune infra de host. Sinon (on-prem, stdio, réseau interne) → auto-hébergé.

## Voir aussi

- [[mcp-options-architecture]]
- [[mcp-transports]]
