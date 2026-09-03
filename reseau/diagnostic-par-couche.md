---
tags: [reseau, diagnostic, outils, troubleshooting]
---

# Diagnostiquer en descendant la pile

L'ordre importe autant que les outils : suivre **nom → route → port → TLS → HTTP** évite de chercher une cause de couche 7 alors que la couche 3 est déjà cassée.

## Un outil par question

| Question | Couche | Commande |
|----------|--------|----------|
| Le nom résout-il, et vers quoi ? | 7 | `dig +short api.exemple.com`<br>`Resolve-DnsName api.exemple.com` |
| Quel résolveur ai-je interrogé ? | 7 | `dig api.exemple.com \| grep SERVER` |
| La machine est-elle joignable ? | 3 | `ping <ip>` — *voir avertissement* |
| Par où passent mes paquets ? | 3 | `traceroute -T -p 443 <hote>`<br>`tracert <hote>` |
| Le port est-il ouvert ? | 4 | `Test-NetConnection <hote> -Port 443`<br>`nc -zv <hote> 443` |
| Qui écoute chez moi, sur quelle interface ? | 4 | `ss -tlnp`<br>`netstat -ano \| Select-String ":443"` |
| Le certificat est-il valide ? | 6 | `openssl s_client -connect h:443 -servername h` |
| Que répond vraiment le serveur ? | 7 | `curl -v https://...` |
| Que circule-t-il sur le fil ? | 2 | `tcpdump -ni any host <ip> and port 443` |

## Ne jamais conclure d'un ping sans réponse

ICMP est filtré par défaut sur la plupart des firewalls d'entreprise et des services cloud, **Azure compris**. Un hôte parfaitement sain y reste muet.

> Pour tester une accessibilité réelle, tester le **port applicatif**, pas l'écho.

D'où `traceroute -T -p 443` (mode TCP) plutôt que le traceroute ICMP par défaut.

## Lire une trace curl

```
*  Trying 93.184.216.34:443       <- couches 3/4 : resolution + TCP
*  SSL certificate verify ok       <- couche 6 : TLS
> GET /v1/users HTTP/2             <- couche 7 : requete
< HTTP/2 200                       <- couche 7 : reponse
```

Les lignes `*` sont les commentaires de curl sur l'établissement de connexion, `>` et `<` le trafic HTTP réel. **Savoir jusqu'où la trace progresse localise la panne à la couche près.**

## Voir aussi

- [[erreurs-connexion-econnrefused]]
- [[mtu-fragmentation]]
- [[openssl]]
- [[Modèle OSI]]
