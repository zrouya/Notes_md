---
tags: [observability, grafana, plugins, go, react]
---

# Structure d'un plugin Grafana

Un plugin = un dossier (souvent `/var/lib/grafana/plugins/<id>`) lu par Grafana au démarrage.

## Arborescence

```
mon-plugin/
├── plugin.json        ← manifeste (obligatoire)
├── module.js          ← frontend compilé (React/TS)
├── img/               ← logo, captures
├── README.md / CHANGELOG.md
├── dashboards/        ← (app) dashboards fournis
├── gpx_<os>_<arch>    ← (optionnel) binaire backend Go
└── MANIFEST.txt       ← signature
```

## Composants

- **`plugin.json`** : id, `type` (`datasource` / `panel` / `app`), version, compatibilité Grafana, routes, pages, dépendances
- **Frontend `module.js`** : React/TypeScript avec `@grafana/data`, `@grafana/ui`, `@grafana/runtime`
  - datasource → éditeur de config (URL, auth) + éditeur de requête
  - panel → composant de rendu + options
- **Backend Go (optionnel)** : Grafana Plugin SDK, processus séparé, dialogue en **gRPC**. Indispensable pour :
  - les **alertes** (évaluées côté serveur, sans navigateur)
  - garder les **secrets** côté serveur (chiffrés, jamais envoyés au navigateur)
  - protocoles non HTTP, cache, traitements lourds
  - expressions, requêtes publiques

## Signature

Niveaux : `grafana`, `commercial`, `community`, `private` (usage interne). Un plugin non signé ne charge que si listé dans `allow_loading_unsigned_plugins` (`grafana.ini`).

## Démarrer un plugin

```bash
npx @grafana/create-plugin@latest
```

## Voir aussi

- [[grafana-plugins]]
- [[grafana-plugin-installation]]
