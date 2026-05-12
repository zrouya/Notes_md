---
tags: [iac, atmos, go-templates]
---

# Atmos — Go Templates

Atmos supporte les Go templates dans les manifests de stack (quand `templates.settings.enabled: true`).

## Ce qui fonctionne ✅

Références directes à des vars simples dans les **vars de composant** :

```yaml
components:
  terraform:
    webapp/defaults:
      vars:
        web_app_name: "ent-az-we-{{ .vars.environment }}-pfs-{{ .vars.domain }}-app-{{ .vars.resource_identifier }}"
```

Les fonctions Sprig (`printf`, etc.) avec `templates.settings.sprig.enabled: true` :

```yaml
web_app_name: '{{ printf "ent-az-we-%s-pfs-%s-app-%s" .vars.environment .vars.domain .vars.resource_identifier }}'
```

### Dériver une var de composant depuis une autre var

Pattern utile pour "aliaser" une var dans un composant abstrait/défaut, à partir d'une var définie dans les stacks enfants :

```yaml
# Dans un _defaults.yaml (composant abstrait ou concret)
components:
  terraform:
    apimapi:
      vars:
        # version_set_name prendra la valeur de api_name, définie dans la stack enfant
        version_set_name: "{{ .vars.api_name }}"
        version_set_display_name: "{{ .vars.display_name }}"
```

`.vars` est le contexte **final mergé** du composant — il inclut les vars globales de toutes les stacks de la chaîne d'import, y compris les stacks enfants qui ont importé ce fichier.

## Ce qui ne fonctionne pas ❌

### Vars intermédiaires contenant des templates (double-pass)

```yaml
# ❌ Ne fonctionne PAS
vars:
  resource_name: "ent-az-we-{{ .vars.environment }}-..."  # stocké comme littéral

components:
  terraform:
    webapp:
      vars:
        web_app_name: "{{ .vars.resource_name }}"  # retourne le littéral, non rendu
```

**Raison** : le rendu est single-pass. `.vars.resource_name` retourne la chaîne brute, sans re-rendu.

### Blocs `{{ define }}` cross-fichiers

```yaml
# ❌ Ne fonctionne PAS entre fichiers
{{- define "azure.resource_name" -}}...{{- end -}}
```

Chaque fichier YAML est traité dans un contexte template **isolé**. Les `define` d'un fichier ne sont pas visibles depuis un autre.

### Templates dans les vars globales (top-level)

```yaml
# ❌ Ne fonctionne PAS
vars:
  resource_group_name: "ent-az-we-{{ .vars.environment }}-..."
```

Les vars globales ne sont pas rendues comme Go templates.

### Valeur YAML commençant par `{{`

```yaml
# ❌ Casse le parsing YAML
environment: {{ .vars.environment }}

# ✅ Fonctionne (chaîne quotée)
environment: "{{ .vars.environment }}"

# ✅ Fonctionne (dans un contexte string)
resource_name: "prefix-{{ .vars.environment }}-suffix"
```

## Contexte disponible

- `.vars` — vars mergées du composant
- `.atmos_component`, `.atmos_stack` — métadonnées
- `.settings`, `.env`, `.providers`

## Fichiers `.yaml.tmpl`

Pour les fichiers contenant des directives Go templates pures (ex: blocs `{{ define }}`), renommer en `.yaml.tmpl`. Atmos **rend le template en premier** (substitution de texte), puis parse le YAML résultant.

**Limite** : les `define` restent locaux au fichier, même en `.yaml.tmpl`.

### Conséquence sur le contenu multilignes

Le rendu template-first crée un piège avec les blocs scalaires YAML `|` :

```yaml
# ❌ Casse le YAML après rendu si le fichier est multilignes
swagger_content: |
  {{ file.Read "stacks/myapp/swagger.json" }}
```

Seule la 1ère ligne hérite de l'indentation du template ; les lignes suivantes sont à col 0, ce qui sort du bloc scalaire → erreur YAML `did not find expected key`.

```yaml
# ✅ Chaîne single-quoted : { est littéral, pas un flow mapping
swagger_content: '{{ file.Read "stacks/myapp/swagger.json" }}'
```

À l'intérieur de `'...'`, YAML traite `{` comme du texte brut. Atmos rend ensuite le template Go et la valeur finale est le contenu du fichier (multilignes compris).

### validate vs describe

| Commande | Comportement |
|---|---|
| `atmos validate stacks` | Valide le YAML brut (avant rendu) — les `{{ }}` dans `'...'` passent |
| `atmos describe stacks` | Rend les templates puis parse le YAML — teste le résultat final |

## Voir aussi

- [[atmos-overview]]
- [[atmos-component-function]]
- [[atmos-terraform-output]]
- [[atmos-datasources]]
