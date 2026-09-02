---
tags: [mcp, transport, stdio, http, sse]
---

# MCP — Transports (stdio et Streamable HTTP)

Deux transports standard. La couche message (JSON-RPC 2.0) est identique dans les deux cas — le transport n'est qu'un détail d'acheminement.

## stdio

Le host lance le serveur en **sous-processus** et échange des messages JSON-RPC délimités par newline sur stdin/stdout.

- Pour : serveurs locaux (fichiers, git, CLI wrappé, base locale)
- Avantages : aucune configuration réseau, pas d'authentification à gérer, latence minimale
- **Piège majeur** : `stdout` est réservé au protocole. Tout log doit partir sur `stderr`, sinon la session casse.

## Streamable HTTP

Un seul endpoint (ex. `/mcp`) qui accepte des `POST` JSON-RPC. La réponse est soit du JSON simple, soit un **flux SSE** quand le serveur veut pousser plusieurs messages (progression, notifications, sampling).

- Pour : serveurs distants, multi-utilisateurs, SaaS
- Session identifiée par l'entête `Mcp-Session-Id`
- Extinction propre : `DELETE` sur l'endpoint pour libérer la session côté serveur

## HTTP+SSE (déprécié)

L'ancien transport à **deux endpoints séparés** est déprécié. À ne plus implémenter pour du neuf ; à garder seulement en compatibilité descendante.

## Conséquence pour le choix d'architecture

stdio exige que **quelqu'un lance un processus sur la machine**. C'est pour ça que le MCP connector de l'API Claude ne supporte que HTTP : personne chez Anthropic ne va exécuter un binaire sur ton poste.

## Voir aussi

- [[mcp-roles-host-client-server]]
- [[mcp-handshake-initialize]]
- [[mcp-authentification]]
- [[mcp-connector-api-claude]]
