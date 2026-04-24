---
tags: [azure, identity, managed-identity, iam]
---

# System-Assigned Managed Identity

Identité managée créée automatiquement par Azure et liée au cycle de vie d'une ressource (VM, App Service, Function App, etc.).

## Caractéristiques

- Créée et supprimée **automatiquement** avec la ressource parente
- **Non partageable** : 1 identité = 1 ressource
- Les credentials sont gérés et renouvelés par Azure (rotation automatique)
- Aucune ressource Azure distincte n'est créée

## Activation (exemple ARM / Bicep)

```json
{
  "identity": {
    "type": "SystemAssigned"
  }
}
```

## Cas d'usage

- Workloads isolés où chaque ressource doit avoir ses propres permissions
- Ressources éphémères (scale-out/in fréquent)
- Contextes où le partage d'identité entre ressources est indésirable

## Voir aussi

- [[managed-identity-user-assigned]]
- [[managed-identity-comparaison]]
