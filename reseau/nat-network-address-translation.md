---
tags: [reseau, nat, adressage, generique]
---

# NAT (Network Address Translation)

Un NAT **réécrit les adresses** des paquets qui le traversent, et note chaque traduction dans une **table** pour pouvoir défaire la traduction au retour. Usage principal : permettre à des machines en [[ip-publique-vs-privee|IP privée]] de partager une IP publique pour sortir sur Internet, ce qui économise les adresses IPv4.

> [!abstract] Portée : notion générique (réseau)
> Déclinaisons Azure : [[sortie-internet-nat-gateway|NAT Gateway]], le SNAT d'Azure Firewall et d'Azure Load Balancer (voir [[ports-ephemeres-time-wait]]).

## Aller et retour

```
Aller   192.168.1.10:51234 → 93.184.216.34:443
  NAT → 203.0.113.5:40001  → 93.184.216.34:443      (source réécrite)

Retour  93.184.216.34:443  → 203.0.113.5:40001
  NAT → 93.184.216.34:443  → 192.168.1.10:51234     (destination remise)
```

Table de traduction créée à l'aller :

| Interne | Externe (traduit) | Destination | Proto |
|---------|-------------------|-------------|-------|
| `192.168.1.10:51234` | `203.0.113.5:40001` | `93.184.216.34:443` | TCP |

Au retour, le NAT cherche la ligne qui correspond au **port traduit** (`40001`), à l'interlocuteur et au protocole. Il remet l'adresse d'origine, puis le paquet est [[Routage réseau|routé]] normalement. C'est le port qui distingue les postes partageant la même IP publique.

## Variantes

| Variante | Ce qui est traduit | Usage |
|----------|--------------------|-------|
| NAT statique 1:1 | L'IP seule | Exposer une machine précise |
| NAT dynamique | L'IP, prise dans un pool | Rare aujourd'hui |
| **PAT / NAPT** (« masquerade », « overload ») | IP **et port** | Le cas courant : box, NAT Gateway, firewall |

- **SNAT** (*Source NAT*) : réécrit la source à l'aller. Sortie vers Internet.
- **DNAT** (*Destination NAT*) : réécrit la destination à l'aller. Redirection de port (`203.0.113.5:8443` → `192.168.1.20:443`).

## Conséquences

- **Seul l'intérieur ouvre une connexion** : un paquet entrant sans ligne dans la table est jeté. Pour l'inverse, il faut un DNAT.
- **Les lignes expirent** : à la fermeture TCP (FIN/RST) ou après inactivité. Une connexion longue et silencieuse peut être coupée : prévoir des *keepalives*. En UDP, la ligne ne vit que quelques dizaines de secondes.
- **Ports limités** : environ 64 000 ports par IP publique et par destination. Beaucoup de connexions vers le même serveur épuisent les ports (épuisement SNAT).
- **Le retour doit repasser par le même NAT** : lui seul possède la ligne. Même logique que le [[filtrage-stateful-stateless|filtrage stateful]] et le [[routage-asymetrique]].
- **La source réelle disparaît** : le destinataire voit l'IP du NAT. Un filtrage par IP source en aval ([[acl-liste-controle-acces|ACL]], NSG) ne distingue plus les émetteurs.
- Ce n'est **pas** une mesure de sécurité en soi, même si l'effet « pas d'entrée non sollicitée » y ressemble.

## Voir la table

```bash
sudo conntrack -L   # Linux : connexions suivies, tuple d'origine et tuple de retour
```

## Voir aussi

- [[ip-publique-vs-privee]] · [[acheminement-ip-publique-privee]]
- [[plages-ip-reservees]] : dont le CGNAT `100.64.0.0/10`
- [[ports-ephemeres-time-wait]] · [[filtrage-stateful-stateless]]
- [[sortie-internet-nat-gateway]] : l'équivalent Azure
