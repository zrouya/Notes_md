
Synthèse de la stratégie d'un POC d'observabilité extensible à un **SI hybride** (Azure + OnPrem dont Docker Swarm), **multi-stack** (.Net, Java, PHP), avec de nombreux services.

## Objectif

Une observabilité **fine, intégrée, extensible à tout le SI**, en minimisant la dépendance au **prestataire infogéreur** de l'OnPrem (dont toute action réseau/hôte implique une validation lente).

## Décision structurante

- **[[OpenTelemetry]] = fondation non négociable.** Le contexte hybride + hétérogène le rend obligatoire : instrumenter une fois, corréler par `trace_id` quel que soit le langage. La vraie décision restante = le **backend** et **où l'héberger**.
- **Backend neutre self-hosté dans Azure** (périmètre maîtrisé, pas d'egress vers un SaaS externe → évite la validation prestataire). [[Vendor lock-in|Lock-in]] faible sur la partie coûteuse (le code).

## Le levier clé : l'APIM comme chokepoint

Tous les appels inter-services (Azure **et** OnPrem) transitent par l'**Azure APIM**. C'est un **point d'observation unique, déjà sous contrôle**, qui voit aussi le trafic OnPrem → base démontrable **sans toucher à l'OnPrem ni au prestataire**.

## Architecture en couches (découplage progressif du prestataire)

| Couche | Quoi | Dépendance | Apport |
|---|---|---|---|
| **0 — Gateway** | Télémétrie native APIM → Log Analytics → Grafana | Aucune (autonome) | RED, latence gateway/backend, service map — **boîte noire** de tout le SI |
| **1 — Apps Azure** | [[OpenTelemetry\|OTel]] sur les apps contrôlées → traces internes propagées via APIM | Autonome | **Boîte blanche** : *pourquoi* (SQL, méthode, dépendance) |
| **2 — OnPrem profond** | Instrumentation apps OnPrem + métriques infra | **Prestataire** | Différée jusqu'à valeur prouvée |

Principe : **montrer d'abord (couches 0-1 autonomes), faire valider ensuite** par la hiérarchie sur une base démontrable.

## Couche 0 — plan concret

- Activer diagnostic settings APIM → catégorie **`GatewayLogs`** → Log Analytics (table `ApiManagementGatewayLogs`). Sampling 100 %, **pas de bodies/headers**.
- Platform metrics APIM dispo nativement dans Azure Monitor.
- Déployer **Grafana en Container App** (volume Azure Files + Managed Identity + rôle `Monitoring Reader`), datasource **Azure Monitor** en Managed Identity.
- 4 dashboards : vue d'ensemble SI / RED par API / latence gateway-vs-backend / erreurs & top offenders.
- `TotalTime - BackendTime` = temps passé dans le gateway → localiser la lenteur sans instrumenter le backend.
- **IaC (Terraform)** dès le POC → critère de crédibilité (reproductible rec/prod).

## Couche 1 — instrumentation OTel

- **Java** : `-javaagent:opentelemetry-javaagent.jar` → zéro-code, 100+ libs auto-instrumentées.
- **.Net** : distro / SDK OTel (~quelques lignes), ou automatic instrumentation zéro-code.
- Propagation **W3C `traceparent`** à travers l'APIM (à vérifier/configurer) → recoud spans gateway (couche 0) + spans app (couche 1) en une seule trace.
- Voir [[Cardinalité des métriques]] : assainir les requêtes SQL, sampling maîtrisé.

## Cas des brokers (Azure Service Bus)

- Propagation de contexte async : le `traceparent` est injecté dans les **application properties** du message à la publication, extrait à la consommation → le hop asynchrone est recousu dans la trace.
- **Azure Service Bus** : propagation **native** dans les SDK .Net et Java (meilleur cas, rien à coder).
- Nuance : en batch/fan-out, utiliser des **span links** plutôt que parent-child.
- Le lag/profondeur de file = **métrique** (Azure Monitor), pas trace → complémentaires.
- C'est le hop qui « claque » en démo : impossible à tracer avec du logging maison.

## Découverte des flux = sous-produit de la couche 0

Pas besoin de connaître les flux d'avance : le **service map de la couche 0** révèle les flux réels (hops Azure/OnPrem, latences), qui **désignent** l'app à instrumenter en couche 1.

## Candidat POC idéal

`.Net (Azure, instrumenté) → APIM → Azure Service Bus → Java (OnPrem, javaagent) → backend`
→ empile toutes les dimensions de preuve : cross-stack, cross-boundary, sync **et** async, gateway + spans internes.

Grille de sélection : stacks traversées · hops Azure/OnPrem contrôlés · passe par APIM · branche ASB · douleur métier connue.

## Voir aussi

**Fondations & concepts**
- [[OpenTelemetry]] · [[OpenTelemetry Collector]] · [[Agent OpenTelemetry Azure Container Apps]]
- [[Observabilité - métriques, logs et traces]] · [[Corrélation de traces distribuées]] · [[Cardinalité des métriques]] · [[Vendor lock-in]]

**Briques Azure (couche 0)**
- [[Log Analytics]] · [[Application Insights]] · [[APM (Application Performance Monitoring)]] · [[Diagnostic settings Azure Monitor]]
- [[Table ApiManagementGatewayLogs]] · [[Tables AppRequests et AppDependencies]] · [[Coût de Log Analytics]]

**Visualisation & backends neutres**
- [[Grafana]] · [[Grafana Tempo]] · [[Stack LGTM]]
