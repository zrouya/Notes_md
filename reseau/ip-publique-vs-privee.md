---
tags: [reseau, ip, adressage, nat]
---

# IP publique vs IP privée

Aucune différence de **format** : rien dans le paquet ne dit qu'une adresse est privée. La différence tient à une **convention** (plages réservées) et au **routage** (qui sait l'acheminer).

## Comparaison

| | IP publique | IP privée |
|---|---|---|
| Plages | Tout ce qui n'est pas réservé | RFC 1918 (`10/8`, `172.16/12`, `192.168/16`) |
| Unicité | **Mondiale** | Seulement **au sein d'un réseau** |
| Attribution | IANA → RIR (RIPE, ARIN…) → opérateur / cloud | Libre, par l'administrateur du réseau |
| Routage | Annoncée en **BGP**, routée sur Internet | **Filtrée** par les opérateurs, jamais routée sur Internet |
| Joignable depuis | N'importe où sur Internet | Uniquement un réseau qui a une route vers elle (LAN, VNet, peering, VPN) |

Voir [[plages-ip-reservees]] pour la liste complète des plages spéciales.

## Le pont entre les deux : le NAT

Une machine en IP privée qui sort sur Internet voit son **IP source traduite** en IP publique (box, NAT Gateway, firewall, load balancer). Le serveur distant ne voit **que l'IP publique**.

```
10.0.1.4:51234  ──NAT──▶  20.50.x.y:1024  ──Internet──▶  serveur
                                                         (voit 20.50.x.y)
```

Conséquence pratique pour le filtrage (allow-list d'un service public) : on autorise l'**IP publique de sortie** de l'appelant, pas son IP privée. Exception : si le trafic reste privé de bout en bout (VPN, peering, private endpoint), c'est l'IP privée qui est vue.

## À retenir

- Une IP (publique ou privée) désigne une **interface**, pas une application : plusieurs services peuvent partager une IP (distingués par le port ou le `Host` HTTP / SNI TLS).
- Privée ≠ sécurisée : c'est l'**absence de route** qui isole, pas l'adresse elle-même.
- Une IP publique n'est pas forcément ouverte : firewall, NSG, IP restrictions filtrent en amont.

## Voir aussi

- [[plages-ip-reservees]]
- [[acheminement-ip-publique-privee]]
- [[NAT (Network Address Translation)]]
- [[cidr-notation]] · [[IPv4]] · [[Adresse IP]]
