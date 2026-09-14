---
tags: [observability, tracing, correlation, w3c]
---

# Corrélation de traces distribuées

Le modèle qui **recoud une requête à travers plusieurs services** en une seule trace.

## Le trio d'identifiants

| Terme | Rôle |
|---|---|
| **trace_id** (`OperationId` chez App Insights) | Identique pour **toutes** les lignes d'une même transaction |
| **span id** (`Id`) | Identifiant unique de *cette* étape |
| **parent** (`ParentId`) | Le span id du parent → crée la chaîne |

En suivant les `ParentId`, on reconstitue l'arbre (le waterfall).

## Propagation synchrone (HTTP)

Le contexte voyage dans l'en-tête **W3C `traceparent`** (trace_id + span parent + flags). Un gateway comme l'APIM peut le propager aux backends → spans gateway et app dans une seule trace.

## Propagation asynchrone (brokers)

Le `traceparent` est **injecté dans les propriétés du message** à la publication, extrait à la consommation.
- **Azure Service Bus** : propagation **native** dans les SDK .Net/Java.
- En **batch / fan-out** : utiliser des **span links** plutôt qu'un parent-enfant strict.

## Pourquoi ça compte

Transforme des logs isolés (« publié » / « consommé » sans lien) en **une trace unique** — impossible avec du logging maison.

## Voir aussi

- [[observabilite-piliers]]
- [[appinsights-tables-requests-dependencies]]
- [[opentelemetry]]
