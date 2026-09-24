---
tags: [architecture, event-sourcing, base-de-donnees]
---

# Event Store

Base de données **append-only** qui range les événements en flux (un flux par agrégat, ex. `compte-42`) et permet de s'abonner aux nouveaux événements.

## Schéma SQL minimal

```sql
CREATE TABLE events (
    global_position BIGSERIAL PRIMARY KEY,   -- ordre global (projections)
    stream_id       TEXT        NOT NULL,
    version         INT         NOT NULL,    -- ordre dans le flux
    type            TEXT        NOT NULL,
    data            JSONB       NOT NULL,
    metadata        JSONB,                   -- user, correlationId, causationId
    created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (stream_id, version)              -- verrouillage optimiste
);
```

## Opérations

- `Ajouter(stream, événements, versionAttendue)` : échoue si la version a bougé.
- `LireFlux(stream, depuisVersion)` : réhydratation d'un agrégat.
- `LireTout(depuisPosition)` / abonnement : alimente les projections.

## Outils

| Outil | Remarque |
|---|---|
| EventStoreDB (Kurrent) | Dédié ES, abonnements et projections intégrés |
| Marten | Event store et base documentaire sur PostgreSQL (.NET) |
| Axon | Référence Java |
| Kafka | ❌ Pas un event store : pas de lecture efficace par flux, pas de concurrence optimiste native. Bon pour l'intégration |

## Voir aussi

- [[event-sourcing]]
- [[verrouillage-optimiste]]
- [[projection-event-sourcing]]
