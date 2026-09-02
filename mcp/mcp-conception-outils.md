---
tags: [mcp, conception, outils, securite, contexte]
---

# MCP — Principes de conception des outils

Ce qui décide si un agent fonctionne ou non. Le protocole est trivial ; la forme des outils est tout.

## Recherche en deux temps, partout

- `search` → identifiants + extraits courts (bon marché)
- `fetch` → un seul élément complet

**Jamais un outil qui renvoie 50 000 tokens.** C'est ainsi qu'on obtient « du contexte pertinent sans ingérer l'inutile » — dans la forme des outils, pas dans le prompt.

## Les plafonds sont dans l'outil, pas dans le prompt

Cap de lignes, fenêtre temporelle maximale, timeout de requête, `LIMIT` SQL. Et quand on tronque : **le dire à l'agent** et lui indiquer comment restreindre.

Un prompt qui demande « ne fais pas de requête trop large » sera ignoré tôt ou tard ; un `LIMIT` ne l'est jamais.

## Read-only par construction, jamais par instruction

- Un compte SQL en lecture seule
- Un token qui peut commenter et rien d'autre

C'est la seule garantie réelle. La défense structurelle vaut mille fois la défense par prompt.

## La description de l'outil est un prompt

C'est ce que le modèle lit pour décider. Y investir plus que dans le schéma : quand utiliser cet outil, quand ne pas, ce que signifie une réponse vide, un exemple de requête bien formée.

## Discipline sur le nombre d'outils

5 serveurs × 15 outils = 75 définitions injectées à chaque tour. Regarder le *tool search* (`defer_loading`, chargement des définitions à la demande) plutôt que d'empiler les serveurs.

## Injection de prompt : les entrées externes ne sont pas fiables

Tout contenu venant d'un système externe (ticket, mail, page web, commentaire) peut contenir des instructions hostiles. Mitigations, par ordre d'efficacité :

1. **Capacités d'écriture minimales** et aucun outil capable d'exfiltrer
2. Sources en lecture seule au niveau du credential
3. Dans le prompt, délimiter explicitement le contenu externe comme *données à analyser*, pas comme instructions

## Un serveur MCP tiers est un canal d'injection

La description d'un outil arrive dans le prompt du modèle. Ne connecter que ce qu'on contrôle ou qu'on audite.

## Voir aussi

- [[mcp-roles-host-client-server]]
- [[mcp-authentification]]
- [[mcp-options-architecture]]
