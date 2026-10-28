---
tags: [observability, grafana, plugins, api, json]
---

# Plugin Infinity (Grafana)

Data source communautaire générique : interroge **JSON, CSV, XML, GraphQL** sur HTTP(S) sans écrire de plugin.

## Syntaxe / Exemple

```bash
grafana-cli plugins install yesoreyeram-infinity-datasource
```

Dans un panel : *Type* `JSON` → *Source* `URL` → URL de l'API → sélecteur de lignes (`data.items`) + colonnes.

## Détails

- Alternative légère à un plugin [[grafana-plugin-structure|maison]] pour exposer une API REST interne
- Gère auth (Basic, Bearer, OAuth2, API key) côté serveur
- Parsers backend → compatible **alerting**
- Limites : pas de requêtage avancé type langage dédié, pagination/transformations parfois laborieuses

## Voir aussi

- [[grafana-plugins]]
- [[grafana-plugin-installation]]
