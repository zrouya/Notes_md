
Le protocole [[Internet Control Message Procotol (ICMP)|ICMP]] permet d'envoyer un **grand nombre** de **types** de **messages**.

Les plus courants sont :

- *Echo Request, Echo Reply* : Test de la **disponibilité** et le **statut** d'une [[Adresse IP|adresse IP]] de destination (généralement via une commande **ping**).
	Note : certains [[Firewall|firewalls]] sont configurés pour **bloquer** les messages **ICMP Echo Request**, un ping dans ce cas là rendra un **timeout**.  
- *Destination Unreachable* : Envoyé par un [[Routeurs réseaux|routeur]] lorsqu'il n'**arrive pas** à **délivrer** un [[Paquets (couche réseau)|paquet]] IP.
- *Source Quench* : Envoyé par un hôte ou un routeur lorsqu'il reçoit **plus de paquets** qu'il ne peut **prendre en charge**. Demande à la source de **réduire** le **rythme d'envoi** des données.
- *Redirect Message* : Envoyé par un routeur pour **communiquer l'adresse** d'un **autre routeur** à qui envoyer le paquet, pour **optimiser le routing**.
- *Time Exceeded* : Envoyé par un routeur si un **paquet** a dépassé son **Time To Live (TTL)** (le **nombre maximal** de routeurs par lesquels il peut transiter).
- *Router Advertisement, Router Solicitation (**IPv6**)* : Les routeur **broadcastent** régulièrement leur **adresse IP** sur le réseau via les messages **Router Advertisement**, et inversement les **hôtes** peuvent **solliciter** les routeurs en **broadcastant** un message **Router Soliciation**, et en **attendant une réponse** d'un routeur sous la forme d'un message **Router Advertisement**. 