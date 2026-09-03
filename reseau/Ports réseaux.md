---
tags: [reseau, tcp, udp, ports]
---

# Ports réseaux

L'[[Adresse IP|adresse IP]] mène à la **machine**, le port mène au **processus**. Un port est un entier sur **16 bits** → 65 535 valeurs.

Le couple `IP:port` s'appelle un **socket**.

## Les trois plages

| Plage | Nom | Usage |
|-------|-----|-------|
| 0 – 1023 | Well-known | Services standards — **privilégiés** |
| 1024 – 49151 | Registered | Applications déclarées (5432, 6379…) |
| 49152 – 65535 | Dynamiques | Ports source [[ports-ephemeres-time-wait\|éphémères]] |

Sous Unix, seul **root** peut se lier à un port < 1024. C'est pourquoi un conteneur applicatif écoute sur `8080` et se fait exposer en `80` par l'orchestrateur, plutôt que de tourner en root.

## Ports à connaître

| Port | Service | Port | Service |
|------|---------|------|---------|
| 22 | SSH | 443 | HTTPS · HTTP/3 (UDP) |
| 53 | DNS (**UDP + TCP**) | 1433 | SQL Server |
| 80 | HTTP | 3306 | MySQL |
| 123 | NTP (UDP) | 5432 | PostgreSQL |
| 389 / 636 | LDAP / LDAPS | 6379 | Redis |

## Le quadruplet

Une connexion TCP est identifiée de façon **unique** par : IP source, port source, IP destination, port destination.

C'est ce qui permet à mille clients de joindre le même `:443` sans se mélanger — chacun utilise un port source éphémère différent.

## Vérifier qui écoute

```bash
ss -tlnp                                  # Linux : ports en ecoute + process
netstat -ano | Select-String ":443"       # Windows
```

Attention à l'**interface** de bind : `127.0.0.1:8080` n'est joignable que localement, `0.0.0.0:8080` l'est depuis l'extérieur. C'est la cause n°1 de [[erreurs-connexion-econnrefused|ECONNREFUSED]].

## Voir aussi

- [[Protocole TCP]] · [[Protocole UDP]]
- [[ports-ephemeres-time-wait]]
- [[Couche de transport du modèle OSI]]
