
Les **diagnostic settings** sont le mécanisme générique d'**Azure Monitor** pour **exporter les logs et métriques d'une ressource** vers une destination.

## Principe

- S'applique à **n'importe quelle ressource** Azure (pas propre à l'APIM).
- Destinations possibles : **[[Log Analytics]]**, Storage Account, ou Event Hub.
- On sélectionne des **catégories de logs** propres à la ressource (ex. `GatewayLogs` pour l'APIM) et éventuellement `AllMetrics`.

## À ne pas confondre

Le terme « diagnostic » est surchargé sur Azure :
- **Diagnostic settings** (Azure Monitor) = *router* les logs vers un magasin.
- Une **entité `diagnostic` interne** à certaines ressources (ex. le diagnostic APIM `applicationinsights` / `azuremonitor`) = *façonner* le contenu (sampling, verbosité).

Les deux sont complémentaires : l'un route, l'autre calibre.

## Exemple (APIM)

Activer la catégorie `GatewayLogs` → Log Analytics alimente la table [[Table ApiManagementGatewayLogs]].

## Voir aussi

- [[Log Analytics]]
- [[Table ApiManagementGatewayLogs]]
