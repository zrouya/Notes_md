---
tags: [observability, architecture, lock-in, concepts]
---

# Vendor lock-in

Le **vendor lock-in** (verrouillage fournisseur) = le degré de **dépendance à un fournisseur** et le **coût de le quitter**. Plus migrer ailleurs est cher/douloureux, plus le lock-in est fort.

Ce n'est **pas binaire** : il existe à plusieurs niveaux, de coûts très différents.

## Niveaux (exemple observabilité)

- **Instrumentation — le plus coûteux à défaire.** Un SDK propriétaire oblige à **remodifier le code de toutes les apps** pour changer de backend. C'est ce qu'[[opentelemetry|OpenTelemetry]] évite.
- **Requêtes / dashboards** — KQL, Workbooks, règles d'alerte sont spécifiques : partir = les **réécrire**. Grafana + PromQL sont portables.
- **Données** — l'historique déjà stocké reste au format du fournisseur.

## À retenir

Avec la bonne architecture (OTel + éventuellement [[grafana|Grafana]] par-dessus), on utilise du **service managé** tout en gardant un lock-in **faible sur la partie coûteuse** (le code), et on l'accepte là où c'est peu coûteux (les dashboards).

## Voir aussi

- [[opentelemetry]]
- [[grafana-tempo]]
