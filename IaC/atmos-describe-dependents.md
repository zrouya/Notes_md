---
tags: [iac, atmos, cli, dépendances]
---

# Atmos — `describe dependents` et `settings.depends_on`

Lister les composants qui **dépendent** d'un composant donné. Nécessite une déclaration explicite.

## Syntaxe / Exemple

```bash
# Lister les composants qui dépendent de sqlServer/defaults
atmos describe dependents sqlServer/defaults -s myapp-dev --format yaml

# Format JSON (défaut)
atmos describe dependents sqlServer/defaults -s myapp-dev
```

## Déclarer une dépendance

Dans le fichier catalog du composant dépendant :

```yaml
# catalog/sqlDatabase/_defaults.yaml
components:
  terraform:
    sqlDatabase/defaults:
      metadata:
        type: abstract
      settings:
        depends_on:
          1:                                   # clé numérique, ordre arbitraire
            component: sqlServer/defaults
            stack: "{stack}"                   # même stack
```

## Limitation importante

`describe dependents` retourne `[]` si **aucun** `settings.depends_on` n'est déclaré.  
La commande ne détecte **pas** automatiquement les dépendances — elle ne fait que lire les déclarations explicites.

## Usage pour l'ordre de déploiement

Pour construire un ordre de déploiement, il faut :
1. Inverser le graphe : lister les `depends_on` de chaque composant (pas ses dépendants)
2. Faire un tri topologique
→ Complexe à implémenter. Préférer les stratégies alternatives décrites dans [[atmos-deploy-ordering]].

## Voir aussi

- [[atmos-deploy-ordering]]
- [[atmos-describe-query]]
