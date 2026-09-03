---
tags: [reseau, tcp, ports, diagnostic]
---

# Ports éphémères et TIME_WAIT

Une connexion TCP est identifiée par un **quadruplet** : IP source, port source, IP destination, port destination. C'est ce qui permet à mille clients de joindre le même `:443` sans se mélanger — chacun tire un port source différent.

## Plage éphémère

| Système | Plage par défaut |
|---------|------------------|
| Linux | 32768 – 60999 (~28 000 ports) |
| Windows | 49152 – 65535 (~16 000 ports) |

```bash
cat /proc/sys/net/ipv4/ip_local_port_range
netsh int ipv4 show dynamicport tcp
```

## TIME_WAIT

À la fermeture, le côté qui ferme **en premier** garde le quadruplet réservé ~2 min (2×MSL). Objectif : éviter qu'un paquet retardataire ne pollue une nouvelle connexion réutilisant le même quadruplet.

## Le problème sous charge

Un service qui ouvre beaucoup de **connexions courtes sortantes** accumule les `TIME_WAIT` et épuise ses ports éphémères.

- Symptôme : `EADDRINUSE`, ou connexions sortantes qui échouent uniquement **sous charge**.
- Sur Azure : le SNAT port exhaustion sur Load Balancer / Container Apps est exactement ce phénomène, avec un quota de ports SNAT par instance.

```bash
ss -tan state time-wait | wc -l      # compter les TIME_WAIT
netstat -ano | find /c "TIME_WAIT"   # equivalent Windows
```

## La vraie correction

**Réutiliser les connexions** plutôt que d'élargir la plage : keep-alive HTTP, pool de connexions, client HTTP unique et partagé (`HttpClient` statique en .NET, `http.Client` réutilisé en Go).

Élargir la plage ou baisser le délai ne fait que repousser le seuil.

## Voir aussi

- [[cout-rtt-connexion-https]]
- [[Ports réseaux]]
- [[Protocole TCP]]
- [[erreurs-connexion-econnrefused]]
