---
tags: [azure, container-apps, serverless, finops, facturation]
---

# Container Apps : le plan Consumption (serverless)

Le plan **Consumption** est le modèle serverless de Container Apps : infrastructure mutualisée gérée par Microsoft, **facturée à la seconde** selon les ressources réellement consommées, avec **scale to zero**.

## Exemple (Bicep)

```bicep
template: {
  containers: [{
    name: 'api'
    image: 'myregistry.azurecr.io/api:1.0'
    resources: { cpu: json('0.5'), memory: '1Gi' }   // ratio 1 vCPU : 2 GiB
  }]
  scale: { minReplicas: 0, maxReplicas: 10 }         // 0 = scale to zero
}
```

## Ce qu'on paie

| Poste | Unité |
|---|---|
| CPU | vCPU-seconde, pour chaque réplica actif |
| Mémoire | GiB-seconde, pour chaque réplica actif |
| Requêtes HTTP | par million |

- **Tarif « idle »** : un réplica qui tourne sans rien traiter (CPU et réseau quasi nuls) est facturé à un prix réduit. C'est le cas typique d'un `minReplicas > 0` la nuit.
- **Réplica arrêté (scale to zero)** : rien à payer.
- **Quota gratuit mensuel, par abonnement** : 180 000 vCPU-s, 360 000 GiB-s et 2 millions de requêtes.

## Caractéristiques

- On ne voit **ni nœud ni VM** : on déclare uniquement le CPU et la mémoire par conteneur.
- **Taille max d'un réplica** : 4 vCPU / 8 GiB.
- Combinaisons CPU/mémoire **imposées** (ratio 1:2, de `0.25`/`0.5Gi` à `4`/`8Gi`).
- Le scaling est piloté par des règles KEDA : HTTP, file d'attente, cron, CPU…
- **Cold start** : avec `minReplicas: 0`, la première requête attend le démarrage d'un réplica. Pour une API sensible à la latence, mettre `minReplicas: 1`, ce qui reste peu coûteux grâce au tarif idle.

## Deux sens du mot « Consumption »

1. **Environnement *Consumption only*** : l'ancien type d'environnement, qui ne fait que du serverless.
2. **Profil `Consumption`** : le profil serverless intégré à un environnement *Workload profiles*.

La facturation est la même dans les deux cas. Ce sont les capacités réseau de l'environnement qui diffèrent, voir [[container-apps-workload-profiles]].

## Quand le Consumption ne suffit plus

- Besoin de plus de 4 vCPU / 8 GiB par réplica.
- Charge stable 24/7 : un profil Dedicated peut revenir moins cher.
- Besoin d'isolation matérielle ou de GPU dédié.

On ajoute alors un profil **Dedicated**, ce qui n'est possible que dans un environnement *Workload profiles*.

## Voir aussi

- [[container-apps-workload-profiles]]
- [[container-apps-choix-irreversibles]]
- [[container-apps-logs-finops]] : l'autre gros poste de coût
