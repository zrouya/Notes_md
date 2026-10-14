---
tags: [reseau, securite, firewall, acl, generique]
---

# ACL réseau (liste de contrôle d'accès)

Une ACL est une **liste ordonnée de règles « autoriser / refuser »** qu'un équipement réseau applique à chaque paquet, selon ses adresses, ses ports et son protocole.

> [!abstract] Portée : notion générique (réseau)
> Déclinaison Azure : le [[network-security-group-nsg-azure|NSG]] est une ACL stateful posée sur un subnet ou une carte réseau.

## Exemple

```
#   Source            Destination       Proto  Port dst  Action
10  192.168.0.0/16    10.3.1.0/26       TCP    443       permit
20  10.1.0.0/23       10.3.1.0/26       TCP    443       permit
99  any               any               any    any       deny    # refus implicite
```

## Détails

- Une règle compare le **5-tuple** : IP source, IP destination, port source, port destination, protocole. Couches 3 et 4 du [[Modèle OSI]], sans lire le contenu applicatif.
- Les règles sont lues **dans l'ordre** : la **première qui correspond s'applique**, les suivantes sont ignorées.
- Une ACL se termine presque toujours par un **refus implicite** de tout le reste.
- On l'applique dans un **sens** (entrée ou sortie) et sur une **interface** donnée.
- Une ACL peut être **stateless** (ACL de routeur classique : il faut écrire aussi le retour) ou **stateful** (elle laisse passer les réponses), voir [[filtrage-stateful-stateless]].
- Un refus est souvent un `DROP` silencieux : le client voit un **timeout**, voir [[Firewall]].

## ACL ou firewall ?

| | ACL | Firewall |
|--|-----|----------|
| Critères | IP, ports, protocole | Idem, plus noms de domaine, applications, contenu |
| Emplacement | Au plus près de chaque segment | Point de passage central |
| Journalisation | Souvent limitée | Systématique |

Les deux se complètent : le firewall contrôle les flux entre zones, l'ACL cloisonne au plus près.

## Voir aussi

- [[Firewall]] · [[filtrage-stateful-stateless]]
- [[Ports réseaux]] · [[cidr-notation]]
- [[network-security-group-nsg-azure]] : l'équivalent Azure
