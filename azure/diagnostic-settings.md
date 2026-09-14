---
tags: [azure, monitoring, diagnostic-settings, log-analytics]
---

# Diagnostic settings (Azure Monitor)

Mécanisme générique d'**Azure Monitor** pour **exporter les logs et métriques d'une ressource** vers une destination.

## Principe

- S'applique à **n'importe quelle ressource** Azure.
- Destinations : **[[log-analytics|Log Analytics]]**, Storage Account, ou Event Hub.
- On sélectionne des **catégories de logs** propres à la ressource (ex. `GatewayLogs` pour l'APIM) et éventuellement `AllMetrics`.

## À ne pas confondre

Le terme « diagnostic » est surchargé sur Azure :
- **Diagnostic settings** (Azure Monitor) = *router* les logs vers un magasin.
- Une **entité `diagnostic` interne** à certaines ressources (ex. le diagnostic APIM `applicationinsights` / `azuremonitor`) = *façonner* le contenu (sampling, verbosité).

L'un route, l'autre calibre.

## Exemple (APIM)

Activer la catégorie `GatewayLogs` → Log Analytics alimente la table [[apim-gateway-logs]].

## Voir aussi

- [[log-analytics]]
- [[apim-gateway-logs]]
