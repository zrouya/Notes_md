---
tags: [azure, reseau, vnet, routage, diagnostic]
---

# Routage asymétrique

Il y a **routage asymétrique** quand l'aller et le retour d'une connexion prennent **des chemins différents**. C'est sans conséquence avec des routeurs simples, mais **fatal** dès qu'un firewall stateful est sur un seul des deux chemins.

## Pourquoi ça casse

Un [[filtrage-stateful-stateless|firewall stateful]] n'accepte un paquet de réponse que s'il a **vu passer l'aller**. S'il ne voit que la moitié de la conversation, il la considère suspecte et la jette.

## Exemple type en hub-and-spoke

```
Spoke A (10.1.0.0/16) : UDR  10.2.0.0/16 -> Firewall 10.0.1.4
Spoke B (10.2.0.0/16) : PAS d'UDR (oubli)

Aller  : VM-A ──> Firewall ──> VM-B        firewall voit le SYN
Retour : VM-B ──────────────> VM-A          direct ?!
```

Ici, le retour ne peut même pas partir en direct, puisque A et B ne sont pas appairés. Mais la même situation se produit dès qu'un chemin direct existe : subnet dans le même VNet, route plus spécifique, route BGP apprise de l'on-prem, etc.

Autre cas classique : VM avec une **IP publique** et une UDR `0.0.0.0/0 → firewall`. Le client entre directement par l'IP publique, et la réponse part vers le firewall, qui n'a jamais vu l'aller.

## Symptômes

- **Timeout**, pas de refus explicite.
- Le ping passe parfois (l'ICMP est traité à part) alors que le TCP échoue.
- Le handshake TCP ne se termine jamais (`SYN` envoyé, jamais de `SYN-ACK` reçu côté client).
- Les logs du firewall montrent des paquets rejetés « hors état » (*invalid state*).

## Règle pour l'éviter

**Symétrie** : si un segment envoie un trafic vers le firewall, l'autre segment doit y renvoyer ses réponses. En hub-and-spoke, mettre **la même logique d'UDR dans tous les spokes**, et vérifier les *Effective routes* des deux côtés.

## Voir aussi

- [[vnet-routage-udr]] · [[hub-and-spoke]]
- [[diagnostic-par-couche]]
- [[Cession TCP]]
