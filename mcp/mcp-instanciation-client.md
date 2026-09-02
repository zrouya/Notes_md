---
tags: [mcp, client, cycle-de-vie, architecture]
---

# MCP — Instanciation et cycle de vie d'un client

Un client MCP n'est **pas un processus** : c'est un objet instancié dans le processus du host. La bonne analogie est une connexion à une base de données — pas « un driver », mais *une connexion par base*.

## Ce que l'objet client détient

- Une référence au **transport** (handle du sous-processus en stdio, session HTTP sinon)
- La version de protocole négociée et les capacités du serveur
- La table de corrélation des **ids JSON-RPC** (apparier réponses et requêtes en vol)
- L'état de session : `Mcp-Session-Id`, catalogues en cache, abonnements aux ressources

## Chronologie

1. **Démarrage du host** → lecture de la config → N déclarations de serveurs
2. **Pour chaque déclaration** : construction d'un transport + d'un client, appel de `connect()` ← *moment de l'instanciation*
3. **Handshake** par client, généralement en parallèle
4. **Découverte** : `tools/list` sur chaque client. Le host **préfixe** les noms (`mcp__filesystem__read_file`) — sans ça, deux serveurs exposant un outil `search` rendraient le routage impossible
5. **Fusion** en un seul tableau `tools` envoyé au modèle
6. **Exécution** : `tool_use` du modèle → lecture du préfixe → routage vers le bon client → `tools/call`
7. **Extinction** : stdio → fermer stdin, attendre la sortie, `SIGTERM` si besoin. HTTP → `DELETE` de la session

## Immédiat ou paresseux

- **Immédiat** (Claude Code) : tout connecté au démarrage. Catalogue complet dès le premier tour, au prix d'un temps de lancement proportionnel au nombre de serveurs.
- **Paresseux** : connexion à la première utilisation. Démarrage rapide, mais le modèle ignore l'existence de l'outil tant qu'il n'est pas listé.

Le compromis se paie d'un côté ou de l'autre.

## Pièges

- **Chaque instance de host a ses propres sous-processus.** Deux fenêtres Claude Code sur le même projet lancent *deux* serveurs. Visible immédiatement si le serveur prend un verrou exclusif (lock, port, session navigateur).
- **Le client est bon marché, le serveur pas forcément.** Instancier l'objet coûte des microsecondes ; ce qui coûte c'est le `npx` qui télécharge un paquet ou le handshake OAuth. Le temps de démarrage est là, pas dans le protocole.

## Voir aussi

- [[mcp-roles-host-client-server]]
- [[mcp-handshake-initialize]]
- [[mcp-configuration-claude-code]]
