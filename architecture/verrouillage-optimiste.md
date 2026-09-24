---
tags: [architecture, concurrence, base-de-donnees, event-sourcing]
---

# Verrouillage optimiste

Gérer la concurrence sans verrou : on écrit en indiquant la **version attendue**, et l'écriture échoue si quelqu'un d'autre a modifié la donnée entre-temps.

## Scénario

```
Req A lit compte-42 (v7)        Req B lit compte-42 (v7)
Req A ajoute, versionAttendue=7 → OK, flux en v8
                                Req B ajoute, versionAttendue=7 → CONFLIT
                                Req B recharge (v8), réessaie ou abandonne
```

## Syntaxe

```sql
-- CRUD classique : colonne version
UPDATE comptes SET solde = @s, version = version + 1
WHERE id = @id AND version = @versionLue;   -- 0 ligne = conflit
```

```csharp
// EF Core
[ConcurrencyCheck] public int Version { get; set; }   // ou [Timestamp] rowversion
// → DbUpdateConcurrencyException en cas de conflit
```

En Event Sourcing : contrainte `UNIQUE (stream_id, version)` dans l'[[event-store]].

## Détails

- Adapté quand les conflits sont **rares** ; sinon verrouillage pessimiste (`SELECT ... FOR UPDATE`).
- Réessai automatique possible si la commande reste valide sur le nouvel état.
- Côté HTTP : `ETag` + `If-Match` → `412 Precondition Failed`.

## Voir aussi

- [[agregat-ddd]]
- [[deadlocks]]
