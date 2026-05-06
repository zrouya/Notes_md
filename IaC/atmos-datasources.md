---
tags: [iac, atmos, gomplate, go-templates, json, xml]
---

# Atmos — Injecter du contenu de fichier dans des vars de stack

Deux approches pour lire un fichier `.json` ou `.xml` et injecter son contenu dans les variables d'un composant Atmos.

## Prérequis : activer gomplate dans `atmos.yaml`

```yaml
templates:
  settings:
    enabled: true
    sprig:
      enabled: true
    gomplate:
      enabled: true
```

---

## Option 1 — Datasource déclarée dans `atmos.yaml` (réutilisable)

**`atmos.yaml`** :

```yaml
templates:
  settings:
    gomplate:
      enabled: true
      datasources:
        apim-policy:
          url: "file://./stacks/catalog/apimapi/policy.json"
```

**Stack YAML** :

```yaml
vars:
  # Objet parsé (accès aux champs)
  policy: '{{ datasource "apim-policy" }}'

  # Champ spécifique
  policy_name: '{{ (datasource "apim-policy").name }}'

  # Sérialisation JSON → string
  policy_raw: '{{ datasource "apim-policy" | toJSON }}'
```

Idéale quand le même fichier est partagé entre plusieurs stacks.

---

## Option 2 — `readFile` inline (chemin dynamique)

```yaml
vars:
  # JSON parsé → objet
  policy: '{{ readFile "./stacks/catalog/apimapi/policy.json" | fromJson }}'

  # Contenu brut → string
  policy_raw: '{{ readFile "./stacks/catalog/apimapi/policy.json" }}'

  # Chemin construit depuis une var de stack
  env_config: '{{ readFile (printf "./configs/%s.json" .vars.environment) | fromJson }}'
```

Le chemin est relatif au `base_path` déclaré dans `atmos.yaml` (généralement `./`).

---

## XML

```yaml
vars:
  # Valeur d'un nœud XPath
  conn_string: '{{ datasource "app-config" | xmlPath "//connectionString" }}'
```

---

## Fonctions de conversion disponibles

| Fonction | Description |
|---|---|
| `readFile "path"` | Lit le fichier comme string |
| `fromJson` | Parse une string JSON → objet |
| `toJSON` | Sérialise un objet → string JSON |
| `fromYaml` | Parse une string YAML → objet |
| `xmlPath "xpath"` | Extrait un nœud XML via XPath |

## Voir aussi

- [[atmos-go-templates]]
- [[atmos-overview]]
