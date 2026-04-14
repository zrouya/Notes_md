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

Pour les fichiers contenant des directives Go templates pures (ex: blocs `{{ define }}`), renommer en `.yaml.tmpl`. Atmos skip le parsing YAML initial et rend le template en premier.

**Limite** : les `define` restent locaux au fichier, même en `.yaml.tmpl`.

## Voir aussi

- [[atmos-overview]]
- [[atmos-component-function]]
- [[atmos-terraform-output]]
