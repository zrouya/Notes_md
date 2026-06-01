---
tags: [terraform, opentofu, iac, hcl]
---

# `dynamic` — Blocs imbriqués conditionnels en Terraform/OpenTofu

Génère des blocs imbriqués de façon conditionnelle ou répétée à partir d'une collection.
Équivalent d'un `for` pour les blocs de configuration (pas pour les arguments simples).

## Syntaxe

```hcl
dynamic "nom_du_bloc" {
  for_each = var.ma_liste   # toute collection : list, map, set
  content {
    attribut = nom_du_bloc.value.attribut
  }
}
```

- Si la collection est vide → aucun bloc généré
- L'iterator implicite porte le nom du bloc (`nom_du_bloc.value`)

## Renommer l'iterator

```hcl
dynamic "workload_profile" {
  for_each = var.workload_profiles
  iterator = wp             # → wp.value.name au lieu de workload_profile.value.name
  content {
    name                  = wp.value.name
    workload_profile_type = wp.value.workload_profile_type
  }
}
```

## Différence avec `for_each` sur une ressource

| | `for_each` (ressource) | `dynamic` |
|---|---|---|
| Scope | Duplique la ressource entière | Duplique un sous-bloc |
| Usage | Plusieurs instances d'une ressource | Plusieurs blocs dans une ressource |

## Cas d'usage typiques

Blocs optionnels ou répétables dans les ressources Azure :
`workload_profile`, `ingress`, `traffic_weight`, `container`, `env`, `registry`, `secret`, `identity`

## Exemple concret (module containerAppsEnvironment)

```hcl
resource "azurerm_container_app_environment" "environment" {
  # ...

  dynamic "workload_profile" {
    for_each = var.workload_profiles
    content {
      name                  = workload_profile.value.name
      workload_profile_type = workload_profile.value.workload_profile_type
      minimum_count         = workload_profile.value.minimum_count
      maximum_count         = workload_profile.value.maximum_count
    }
  }
}
```

Si `var.workload_profiles = []` : environnement en mode Consumption (aucun profil).

## Voir aussi

- [[opentofu-import]]
- [[atmos-blueprint-pattern]]
