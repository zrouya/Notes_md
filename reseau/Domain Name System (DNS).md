---
tags: [reseau, dns, resolution]
---

# Domain Name System (DNS)

Annuaire réparti d'Internet. **Aucun paquet ne voyage jamais vers un nom de domaine** : il faut d'abord traduire `api.exemple.com` en [[Adresse IP|adresse IP]].

C'est la première étape de toute connexion — et la première chose qui casse.

## La hiérarchie

Le nom se lit **de droite à gauche**, du plus général au plus précis :

```
api  .  exemple  .  com  .
 |        |         |     └── racine (implicite)
 |        |         └──────── TLD
 |        └────────────────── domaine
 └─────────────────────────── sous-domaine
```

Cette hiérarchie **est** l'organisation de la délégation : chaque niveau ne sait qu'une chose, **à qui demander la suite**.

## Résolution récursive

```
Application → Résolveur récursif (FAI, 8.8.8.8, 1.1.1.1)
                  ├─ (2) racine        → "voici les serveurs .com"
                  ├─ (4) serveurs .com → "voici ns1.exemple.com"
                  └─ (6) ns1           → "A 93.184.216.34"
Application ← réponse + mise en cache (TTL)
```

L'application ne fait **qu'une requête** : tout le travail est porté par le résolveur récursif. En pratique cette cascade est rare — le cache court-circuite tout, tant que le [[dns-ttl-migration|TTL]] n'a pas expiré.

## Transport

**UDP/53**, avec bascule sur **TCP/53** si la réponse dépasse la taille d'un datagramme. Un firewall qui bloque `tcp/53` casse uniquement les zones volumineuses — panne déroutante.

## Vérifier

```bash
dig +short api.exemple.com
dig +trace api.exemple.com      # resolution complete, sans cache
Resolve-DnsName api.exemple.com # Windows
```

## Voir aussi

- [[DNS record]] — types d'enregistrements
- [[dns-ttl-migration]] — TTL et stratégie de bascule
- [[Round Robin DNS]]
- [[diagnostic-par-couche]]
