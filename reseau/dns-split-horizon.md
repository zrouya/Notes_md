---
tags: [reseau, dns, securite, generique]
---

# DNS split-horizon

Un même nom reçoit **une réponse différente selon qui pose la question** : une IP privée depuis le réseau interne, une IP publique (ou rien du tout) depuis Internet. On dit aussi *split-brain DNS*.

> [!abstract] Portée : notion générique (réseau)
> Déclinaison Azure : une [[private-dns-zone|Private DNS Zone]] liée à un VNet produit exactement cet effet, voir aussi [[dns-hybride-azure]].

## Exemple

```
app.exemple.fr
  vu de l'interne  (DNS d'entreprise, zone interne)   → A 10.20.1.15
  vu d'Internet    (DNS public, zone publique)          → A 203.0.113.40   ou NXDOMAIN
```

Deux zones portent le même nom, `exemple.fr` : une interne et une publique. Le résolveur interrogé décide laquelle est utilisée.

## Usages

- Exposer un service en privé à l'interne et en public à l'extérieur, sous **un seul nom**.
- Donner un nom à un service purement interne, sur un domaine public de l'entreprise. Intérêt : une autorité de certification publique peut émettre le certificat, puisqu'on possède le domaine. Tous les clients lui font confiance, même ceux qui ne connaissent pas la PKI interne.

## Pièges

- **Le résolveur doit connaître la zone interne.** Un client qui utilise un DNS public, ou un résolveur qui redirige ce domaine vers Internet, obtient la réponse publique ou `NXDOMAIN`.
- **Zone interne incomplète** : si la zone interne `exemple.fr` existe, le résolveur interne y fait autorité pour **tout** le domaine. Les noms publics absents de la zone interne (`www`, `mail`) deviennent introuvables depuis l'interne. Il faut les recopier, ou ne créer en interne qu'un **sous-domaine** dédié (`int.exemple.fr`).
- **Fuite d'information** : déclarer des noms internes dans la zone publique révèle la cartographie interne.
- **Diagnostic trompeur** : un test depuis son poste (DNS interne) et depuis Internet donnent deux résultats différents, et les deux sont « justes ».

## Vérifier

```bash
nslookup app.exemple.fr              # avec le DNS interne
nslookup app.exemple.fr 1.1.1.1      # avec un DNS public
```

## Voir aussi

- [[dns-roles-resolveur-autorite]] · [[dns-redirection-conditionnelle]]
- [[ip-publique-vs-privee]]
- [[private-dns-zone]] : l'équivalent Azure
