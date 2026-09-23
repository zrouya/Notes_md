---
tags: [azure, entra, identity, rbac, iam]
---

# Entra — Rôles d'annuaire

RBAC sur l'annuaire lui-même, hérité du modèle d'administration Active Directory. Un rôle est un ensemble d'actions `microsoft.directory/...` assigné à un **principal** : utilisateur, groupe, ou service principal.

Répond à : *qu'est-ce que cette identité peut faire dans l'annuaire, quel que soit l'outil utilisé ?*

## Caractéristiques

- Portés par le **principal**, pas par un couple (client, ressource).
- S'appliquent quel que soit le canal : portail, PowerShell, Graph, CLI.
- **Scopables** : tenant entier, unité administrative (AU), ou objet ciblé.
- Assignables en **éligible / time-bound** via PIM.
- Pour un utilisateur, la présence de rôles se reflète dans le claim `wids` du token.
- Granularité : grossière sur l'action, fine sur le périmètre.

## Exemples

| Rôle | Portée typique |
|---|---|
| `Groups Administrator` | Lire/créer/modifier/supprimer **tous** les groupes, membres et owners |
| `Directory Writers` | Écriture limitée sur les objets d'annuaire |
| `Application Administrator` | Gestion des app registrations et service principals |

## Voir aussi

- [[entra-graph-permissions]]
- [[entra-permissions-vs-roles]]
- [[azure-rbac-planes]]
- [[azure-entra]]
