
Les deux tables d'[[Application Insights]] qui portent le tracing distribué.

> **`AppRequests` = les appels *reçus*. `AppDependencies` = les appels *émis*.**

## Le principe

Chaque service instrumenté produit :
- une ligne **`AppRequests`** à chaque requête **reçue** ;
- une ligne **`AppDependencies`** à chaque appel **sortant** (autre service HTTP, SQL, Service Bus, cache…).

Ça s'alterne : *reçu → émis → reçu → émis*. Chaque dépendance émise devient la requête reçue du service suivant.

## Colonnes propres à AppDependencies

- `Target` — l'hôte/ressource distante
- `DependencyType` — `HTTP`, `SQL`, `Azure Service Bus`…
- `Data` — le texte de l'appel (**la requête SQL** assainie, l'URL)
- `Success`, `ResultCode`, `DurationMs`

## Reconstruire une trace

Réunir les deux tables sur un même `OperationId` (voir [[Corrélation de traces distribuées]]) et ordonner par temps → le waterfall.

```kql
union AppRequests, AppDependencies
| where OperationId == "<trace>"
| order by TimeGenerated asc
```

⚠️ Pondérer les comptages par **`ItemCount`** (multiplicateur de sampling).

## vs GatewayLogs

`AppDependencies` porte le détail *« quelle requête SQL, où, en combien de temps »* — ce que [[Table ApiManagementGatewayLogs]] replie dans `BackendTime`.

## Voir aussi

- [[Corrélation de traces distribuées]]
- [[Application Insights]]
