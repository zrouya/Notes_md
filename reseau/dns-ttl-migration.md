---
tags: [reseau, dns, migration, exploitation]
---

# TTL DNS et stratégie de bascule

Le **TTL** d'un enregistrement est la durée pendant laquelle un résolveur garde la réponse en cache. Autrement dit :

> Le TTL est le délai pendant lequel une réponse **périmée** continue de circuler.

## La règle de bascule

Changer un enregistrement à TTL 3600 signifie que des clients pointeront vers l'ancienne IP **pendant une heure**.

1. **J-1** : abaisser le TTL à `60` — plusieurs heures **avant** la bascule, pour laisser l'ancien TTL expirer partout.
2. **Jour J** : changer l'enregistrement. La propagation se fait en ~1 min.
3. **J+1** : remonter le TTL (300–3600) une fois la migration stabilisée.

Sauter l'étape 1 est l'erreur classique : on baisse le TTL et on bascule dans la foulée, alors que les caches portent encore l'ancienne valeur de TTL.

## Ordres de grandeur

| TTL | Usage |
|-----|-------|
| 60 s | Fenêtre de migration, bascule de failover |
| 300 s | Défaut raisonnable pour un service actif |
| 3600 s | Enregistrement stable |
| 86400 s | `NS`, `MX` — change très rarement |

Un TTL trop court a un coût : chaque résolution rate le cache et rajoute un aller-retour, voir [[cout-rtt-connexion-https]].

## Vérifier

```bash
dig api.exemple.com                  # le TTL decroit a chaque interrogation du cache
dig @8.8.8.8 api.exemple.com         # contourner le resolveur local
dig +trace api.exemple.com           # resolution recursive complete, sans cache
```

## Piège firewall

Le DNS utilise **UDP/53**, avec bascule sur **TCP/53** quand la réponse dépasse la taille d'un datagramme. Un firewall autorisant `udp/53` mais bloquant `tcp/53` produit une panne déroutante : la plupart des résolutions passent, **seules les zones volumineuses échouent** (nombreux enregistrements, DNSSEC).

## Voir aussi

- [[Domain Name System (DNS)]]
- [[DNS record]]
- [[Round Robin DNS]]
- [[diagnostic-par-couche]]
