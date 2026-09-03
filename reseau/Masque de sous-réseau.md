---
tags: [reseau, ip, subnet]
---

# Masque de sous-réseau

Combinaison de bits qui divise une [[Adresse IP|adresse IP]] en deux parties : l'identifiant de **réseau** et l'identifiant d'**hôte**.

Le masque ne se transmet jamais dans un paquet — c'est une donnée **locale** à chaque interface, utilisée pour décider où envoyer.

## Principe

Les bits à `1` marquent la partie réseau, ceux à `0` la partie hôte. Ils sont toujours **contigus**, d'où la notation [[cidr-notation|CIDR]] `/n` qui les compte.

```
adresse   192.168.1.10
masque    255.255.255.0    = /24
          11111111.11111111.11111111.00000000
          |______ reseau ___________|_ hote _|
```

## À quoi il sert concrètement

Une machine applique le masque à **sa propre adresse** et à **celle de la destination** :

- résultats **identiques** → même réseau, livraison directe sur le lien
- résultats **différents** → envoi à la [[Passerelle réseau|passerelle]], puis [[Routage réseau|routage]]

C'est ce test, et rien d'autre, qui décide si un paquet sort du réseau local.

## Masques courants

| Masque | CIDR | Hôtes utilisables |
|--------|------|-------------------|
| 255.255.255.255 | `/32` | 1 (route hôte) |
| 255.255.255.248 | `/29` | 6 |
| 255.255.255.0 | `/24` | 254 |
| 255.255.0.0 | `/16` | 65 534 |

Deux adresses par bloc sont toujours réservées (réseau et diffusion) : d'où le `- 2`.

## Voir aussi

- [[cidr-notation]] — calcul détaillé et plages privées
- [[Sous-réseau]] · [[Routage réseau]]
- [[IPv4]]
