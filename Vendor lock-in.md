
Le **vendor lock-in** (verrouillage fournisseur) = le degré de **dépendance à un fournisseur** et le **coût de le quitter**. Plus migrer ailleurs est cher/douloureux, plus le lock-in est fort.

Ce n'est **pas binaire** : il existe à plusieurs niveaux, de coûts très différents.

## Niveaux (exemple observabilité)

- **Instrumentation — le plus coûteux à défaire.** Utiliser un SDK propriétaire (ex. SDK [[Application Insights]]) oblige à **remodifier le code de toutes les apps** pour changer de backend. C'est ce qu'[[OpenTelemetry]] évite : instrumenter une fois avec un standard ouvert, rediriger sans toucher au code.
- **Requêtes / dashboards** — KQL, Workbooks Azure, règles d'alerte sont spécifiques au fournisseur : partir = les **réécrire**. Grafana + PromQL sont portables.
- **Données** — l'historique déjà stocké reste au format du fournisseur.

## À retenir

Avec la bonne architecture (OTel pour instrumenter + éventuellement Grafana par-dessus), on peut utiliser du **service managé** tout en gardant un lock-in **faible sur la partie coûteuse** (le code), et l'accepter là où c'est peu coûteux (les dashboards).

## Voir aussi

- [[OpenTelemetry]]
- [[Application Insights]]
