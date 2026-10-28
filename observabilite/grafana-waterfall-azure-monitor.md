---
tags: [observability, grafana, azure, appinsights, tracing]
---

# Waterfall de traces App Insights dans Grafana

Afficher l'arbre des spans d'une opération [[appinsights-principe|App Insights]] dans [[grafana|Grafana]]. **Azure Managed Grafana suffit** : le panel *Traces* et la datasource Azure Monitor sont intégrés, aucune instance à déployer.

## Configuration du panel

1. **Visualisation** (sélecteur en haut à droite) : **Traces**. Avec *Time series*, la valeur par défaut, on obtient une courbe des temps de requête.
2. **Datasource** : Azure Monitor. **Service** : *Traces*.
3. **Resource** : le composant **Application Insights** (`microsoft.insights/components`), pas le workspace. Plusieurs composants sont sélectionnables si chaque service a le sien.
4. **Operation ID** : obligatoire pour obtenir un arbre. Par exemple une variable `$operationId`.
5. **Formatted as** : **Trace**.
6. **Event types** : tout coché (requests, dependencies, exceptions, traces).

Astuce : un panel *Table* des requêtes lentes, avec un *data link* qui remplit `operationId`, permet de cliquer sur une ligne pour afficher sa waterfall.

## Pourquoi App Insights et pas le workspace

Le mode *Traces* génère un KQL avec le **schéma classique** (`requests`, `operation_ParentId`…), qui n'existe qu'en portée App Insights. Voir [[appinsights-schema-classique-vs-workspace]].
Alternative depuis le workspace : type *Logs*, avec un KQL manuel qui renomme les colonnes en `traceID`, `spanID`, `parentSpanID`, `operationName`, `serviceName`, `startTime`, `duration`. C'est plus fragile. *(À vérifier selon la version de Grafana.)*

## Pas de waterfall : checklist

- Visualisation *Traces* et format *Trace* ?
- L'Operation ID est-il dans la **plage de temps** du dashboard ?
- L'Operation ID a-t-il des **spans** (`AppRequests`/`AppDependencies`) ? Un ID pris dans `AppTraces` peut n'avoir que des logs. Voir [[appinsights-modele-donnees-otel]].
- Le `_ResourceId` des spans correspond-il à la ressource sélectionnée ?
- L'identité managée de Grafana a-t-elle le rôle **Monitoring Reader** ?

## Tester dans Explore

Menu ☰ › **Explore** (icône boussole), ou menu **⋮** du panel › *Explore*, ou l'URL `/explore`. Si ça s'affiche dans Explore mais pas dans le panel, le problème vient de la visualisation.

## Points annexes

- Tier *Essential* suffisant. *Standard* seulement pour les private endpoints, par exemple vers un [[grafana-tempo|Tempo]] privé.
- Pas de recherche de traces façon TraceQL : on cible un ID.
- Le portail Azure offre déjà une waterfall avec *End-to-end transaction details*.

## Voir aussi

- [[grafana]]
- [[appinsights-modele-donnees-otel]]
- [[appinsights-tables-requests-dependencies]]
