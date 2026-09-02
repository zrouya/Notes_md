---
tags: [mcp, json-rpc, handshake, protocole]
---

# MCP — Séquence de connexion et négociation

Séquence identique quel que soit le transport. Trois choses se jouent dans le handshake.

## Séquence

```
Client → initialize        { protocolVersion, capabilities, clientInfo }
Server → result            { protocolVersion, capabilities, serverInfo }
Client → notifications/initialized
--- session ouverte ---
Client → tools/list        (puis resources/list, prompts/list)
Client → tools/call        { name, arguments }
```

## 1. Négociation de version

Le client propose une version de protocole. Si le serveur ne la supporte pas, il répond avec la sienne — le client décide alors de continuer ou d'abandonner.

## 2. Négociation de capacités

Chacun déclare ce qu'il sait faire.

| Côté serveur | Côté client |
|---|---|
| `tools`, `resources`, `prompts`, `logging` | `sampling` (inférence LLM à la demande du serveur) |
| | `roots` (racines de fichiers autorisées) |
| | `elicitation` (saisie utilisateur) |

**Règle** : ne jamais appeler une méthode que l'autre n'a pas déclarée.

## 3. Le flag `listChanged`

Si le serveur l'annonce, il enverra `notifications/tools/list_changed` quand son catalogue bouge — le client doit re-lister. Sans ça, un catalogue mis en cache devient périmé silencieusement.

## Point de cycle de vie

Une session échouée **ne se reprend pas**. Si le transport tombe, on n'a pas de « reconnexion » : on instancie un client neuf avec un `initialize` complet. L'état négocié est perdu.

## Voir aussi

- [[mcp-roles-host-client-server]]
- [[mcp-transports]]
- [[mcp-instanciation-client]]
