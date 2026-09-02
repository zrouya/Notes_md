---
tags: [mcp, architecture, claude-code, agent-sdk, decision]
---

# MCP — Les quatre options d'architecture

Le choix n'est pas binaire entre « utiliser Claude Code » et « tout réimplémenter ». Deux questions indépendantes : **qui fournit le harness** (boucle d'agent + gestion du contexte) et **qui fournit le déploiement**.

## Tableau de décision

| Option | Host | Qui instancie les clients | stdio | Ce que tu écris |
|---|---|---|---|---|
| **Claude Code** | Claude Code, local | Le poste de l'utilisateur | ✅ | Une config |
| **Claude Agent SDK** | Ton app, ton infra | Ton processus | ✅ | Un prompt + des options |
| **Client maison** (SDK MCP + API Claude) | Ton app | Ton processus | ✅ | Boucle d'agent, permissions, contexte |
| **MCP connector** (API Claude) | Serveurs Anthropic | Anthropic | ❌ | Deux champs dans la requête |

## Idée fausse à écarter

**Claude Code ne délègue rien à Anthropic pour MCP.** C'est le binaire local qui lit la config, fait les `fork/exec`, ouvre les sessions HTTP et les handshakes. Les objets clients vivent dans la mémoire du poste. **Aucun paquet JSON-RPC ne transite par une infra Anthropic.**

Ce qui part vers Anthropic : uniquement la requête d'inférence (`/v1/messages`) — prompt, historique, **définitions d'outils** et **résultats d'outils**. Nuance de confidentialité : le contenu renvoyé par un serveur MCP finit bien dans le contexte envoyé au modèle, mais la connexion au serveur est établie depuis le poste.

## En « client maison », tu n'implémentes pas MCP

Les SDK officiels (`@modelcontextprotocol/sdk`, `mcp` en Python) font le handshake, le transport et la corrélation JSON-RPC. Ce que tu écris vraiment, c'est **le host** : boucle d'agent, modèle de permissions, budget de contexte, UI. C'est là qu'est le travail — et la valeur si tu as des exigences propres (audit, autorisation métier, intégration dans une app existante).

## Critère de choix décisif

**Y a-t-il un utilisateur devant un terminal ?**
- Oui → Claude Code
- Non (événement, planification) → job headless `claude -p`, Agent SDK, ou Managed Agents

**Les sources sont-elles joignables depuis Internet ?**
- Non (logs on-prem, bases internes) → auto-hébergé obligatoire

## Voir aussi

- [[mcp-roles-host-client-server]]
- [[mcp-connector-api-claude]]
- [[mcp-configuration-claude-code]]
