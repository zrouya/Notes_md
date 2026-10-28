---
tags: [observability, grafana, plugins, docker]
---

# Installer un plugin Grafana

Trois voies selon l'hébergement de [[grafana]].

## Syntaxe / Exemple

```bash
# Grafana auto-hébergé
grafana-cli plugins install <plugin-id>
# puis redémarrer Grafana
```

```yaml
# Docker / Compose
environment:
  GF_INSTALL_PLUGINS: "yesoreyeram-infinity-datasource,<autre-id>"
```

```ini
# grafana.ini — autoriser un plugin non signé (interne)
[plugins]
allow_loading_unsigned_plugins = mon-org-monplugin-datasource
```

## Détails

- **Auto-hébergé / Docker** : liberté totale, y compris plugins privés ; provisioning possible en IaC
- **Grafana Cloud / Azure Managed Grafana** : installation via le **catalogue** uniquement → choix **restreint**, pas toujours de plugin privé ou non signé
- Un plugin à backend Go embarque un binaire par OS/arch (`gpx_linux_amd64`…) : vérifier la plateforme cible

## Voir aussi

- [[grafana-plugins]]
- [[grafana-plugin-structure]]
