---
tags: [reseau, dns, resolution, generique]
---

# DNS : redirecteurs et redirection conditionnelle

Un résolveur n'est pas obligé de tout chercher lui-même depuis la racine. Il peut **transmettre** certaines questions à un autre serveur DNS. C'est ainsi qu'on relie des DNS qui ne se voient pas naturellement : deux entreprises, l'on-prem et un cloud, une zone interne et Internet.

> [!abstract] Portée : notion générique (réseau)
> Déclinaison Azure : les *forwarding rulesets* et l'*inbound endpoint* du [[dns-private-resolver-azure|DNS Private Resolver]].

## Deux formes

| Forme | Règle | Usage |
|-------|-------|-------|
| **Redirecteur** (*forwarder*) global | « Tout ce que je ne connais pas, demande-le à X » | Envoyer Internet vers le DNS du FAI ou d'un fournisseur |
| **Redirection conditionnelle** (*conditional forwarder*) | « Pour tel domaine, demande à Y » | Joindre une zone gérée ailleurs |

## Exemple

```
Résolveur de l'entreprise
  exemple.local            → je fais autorité (zone locale)
  partenaire.local         → redirection conditionnelle vers 172.16.0.10
  zone-du-cloud.internal   → redirection conditionnelle vers 10.0.0.4
  tout le reste            → redirecteur global vers 1.1.1.1
```

Windows Server : *DNS Manager* → **Conditional Forwarders**. BIND : bloc `zone "x" { type forward; forwarders { ... }; };`.

## Chaînage

Les redirections s'enchaînent : un client interroge A, qui transmet à B, qui transmet à C. Chaque maillon doit pouvoir **joindre** le suivant sur le port 53, en **UDP et TCP**.

## Pièges

- **Boucle** : A redirige un domaine vers B, et B le redirige vers A. Les requêtes tournent jusqu'au timeout.
- **Domaine trop large** : rediriger `exemple.fr` entier alors qu'on ne voulait que `int.exemple.fr` détourne aussi les noms publics.
- **Cible injoignable** : le serveur cible n'est pas routé, ou un firewall bloque le 53. La résolution échoue en timeout (`SERVFAIL`).
- **Cache négatif** : un `NXDOMAIN` obtenu avant la création de la règle reste en cache le temps de son TTL.

## Voir aussi

- [[dns-roles-resolveur-autorite]]
- [[dns-split-horizon]]
- [[dns-private-resolver-azure]] : la mise en œuvre Azure
