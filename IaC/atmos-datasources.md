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

## Option 2 — `file.Read` inline (chemin dynamique)

```yaml
vars:
  # Contenu brut → string (chemin relatif au base_path de atmos.yaml)
  swagger_content: '{{ file.Read "stacks/templates/myapp/projectConfig/swagger.json" }}'

  # Chemin construit depuis une var de stack
  swagger_content: '{{ file.Read (printf "stacks/%s" .vars.swagger_path) }}'
```

> **Chemin** : relatif au `base_path` de `atmos.yaml` (généralement `./`, là où atmos est lancé).

---

## Pattern — Déléguer la lecture dans `_defaults.yaml.tmpl`

Pour centraliser la logique dans le template du domaine plutôt que dans chaque projet :

**`stacks/templates/myapp/projectConfig/sireneApim_dev.yaml`** (projet) :
```yaml
vars:
  swagger_path: templates/myapp/projectConfig/swagger.json
  api_policy_path: templates/myapp/projectConfig/apimPolicy.xml  # optionnel
```

**`stacks/templates/myapp/_defaults.yaml.tmpl`** (template du domaine) :
```yaml
vars:
  api_policy_path: ""   # défaut — override dans le projet si besoin
  swagger_content: '{{ file.Read (printf "stacks/%s" .vars.swagger_path) }}'
  api_policy_xml: '{{ if .vars.api_policy_path }}{{ file.Read (printf "stacks/%s" .vars.api_policy_path) }}{{ end }}'
```

- Les variables `swagger_content` et `api_policy_xml` sont passées au module Terraform.
- La valeur `api_policy_path: ""` garantit que le template ne plante pas si un projet ne définit pas de policy.
- `{{ if .vars.api_policy_path }}` est évalué à `false` sur une string vide → `api_policy_xml = ""`.

---

## ⚠️ Piège : contenu multilignes et parsing YAML

Dans les fichiers `.yaml.tmpl`, Atmos **rend les templates Go en premier** (substitution de texte), puis parse le YAML résultant.

**❌ Bloc scalaire `|` — casse le YAML après rendu :**
```yaml
swagger_content: |
  {{ file.Read "stacks/myapp/swagger.json" }}
```
Après rendu, le contenu multilignes du fichier s'insère à la position du `{{ }}`. Seule la première ligne hérite de l'indentation du template ; les suivantes sont à col 0 → le bloc scalaire YAML est cassé.

**✅ Chaîne single-quoted — fonctionne :**
```yaml
swagger_content: '{{ file.Read "stacks/myapp/swagger.json" }}'
```
À l'intérieur de `'...'`, YAML traite `{` comme un caractère littéral (pas un flow mapping). Atmos rend ensuite le template → la valeur de la variable est le contenu du fichier (multilignes inclus), sans re-parsing YAML.

---

## Fonctions de conversion disponibles

| Fonction | Description |
|---|---|
| `file.Read "path"` | Lit le fichier comme string (gomplate) |
| `readFile "path"` | Alias Sprig (même effet) |
| `fromJson` | Parse une string JSON → objet |
| `toJSON` | Sérialise un objet → string JSON |
| `fromYaml` | Parse une string YAML → objet |

## Voir aussi

- [[atmos-go-templates]]
- [[atmos-overview]]
