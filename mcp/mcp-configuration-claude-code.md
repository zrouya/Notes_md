---
tags: [mcp, claude-code, configuration, deploiement]
---

# MCP — Configuration dans Claude Code

## En CLI

```bash
# stdio (sous-processus local)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /chemin

# HTTP distant
claude mcp add --transport http mon-api https://exemple.com/mcp \
  --header "Authorization: Bearer $TOKEN"

claude mcp list          # état des connexions
```

## En JSON

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/chemin"],
      "env": { "LOG_LEVEL": "warn" }
    },
    "mon-api": {
      "type": "http",
      "url": "https://exemple.com/mcp",
      "headers": { "Authorization": "Bearer ${API_TOKEN}" }
    }
  }
}
```

## Les trois portées

| Portée | Emplacement | Usage |
|---|---|---|
| `project` | `.mcp.json` à la racine du repo | Versionné, partagé par l'équipe |
| `user` | `~/.claude.json` | Serveurs perso, tous projets |
| `local` | Local au projet, non versionné | Expérimentation, secrets |

## Deux points sur la portée `project`

- Le développeur le récupère **en clonant le repo**, pas par un téléchargement séparé
- **Claude Code demande une approbation explicite** au premier lancement. Protection nécessaire : sinon cloner un repo suffirait à faire exécuter un processus arbitraire. Ne pas compter sur un déploiement 100 % silencieux par ce canal.

## La config seule ne suffit pas

Pour qu'un serveur stdio fonctionne chez un utilisateur, il faut aussi :
- le runtime présent (`node`/`npx`, `python`/`uvx`)
- le paquet installable (proxy, registre interne)
- les credentials existants

**C'est là que les déploiements échouent en pratique, pas dans le JSON.**

## Voir aussi

- [[mcp-instanciation-client]]
- [[mcp-transports]]
- [[mcp-options-architecture]]
