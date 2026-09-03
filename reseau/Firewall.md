
Les pare-feu (Firewalls) sont des systèmes de **protection de réseaux**, qui **filtrent le trafic** les traversant (ils filtrent les **paquets de données** transitant par eux).

Ils peuvent être de type **network-based (hardware)** ou **host-based (software)**. 

Il existe 3 générations de firewalls : 

- 1ère génération : les plus **basiques**, ils implémentent des **règles de filtrage simples** basées sur des **white/black list** : [[Adresse IP|adresses IP]] autorisées, ou rejetées.

- 2ème génération : **Circuit-level** firewalls. 
	Ils monitorent les [[Cession TCP|cessions TCP]] valides et invalides

- 3ème génération : **Next Generation Firewalls (NGFW)**
	- **Plus avancés**, ils agissent au niveau de la [[Couche applicative du modèle OSI|couche 7]] du modèle OSI

## DROP ou REJECT — la distinction qui se voit côté client

Face à un paquet refusé, un firewall a deux comportements possibles, et **le client ne voit pas la même erreur** :

| Action | Ce que fait le firewall | Erreur côté client |
|--------|-------------------------|--------------------|
| `REJECT` | Répond un `RST` (TCP) ou un ICMP *unreachable* | `ECONNREFUSED` — **immédiat** |
| `DROP` | Jette le paquet **en silence** | `ETIMEDOUT` — après le timeout |

Conséquences pratiques :

- `DROP` est le défaut sur la plupart des firewalls d'entreprise et des NSG Azure — il ne révèle pas l'existence de la machine, mais fait **attendre** le client jusqu'au timeout.
- Un `ETIMEDOUT` oriente donc vers une **règle réseau** ; un `ECONNREFUSED` oriente vers un **service absent**, pas vers le firewall.

Voir [[erreurs-connexion-econnrefused]] pour la table complète.

## Effet de bord sur le diagnostic

La plupart des firewalls bloquent l'**ICMP** par défaut, Azure compris. Deux conséquences :

- un `ping` sans réponse **ne prouve rien** — tester le port applicatif, voir [[diagnostic-par-couche]]
- bloquer l'ICMP type 3 code 4 casse la découverte de MTU, voir [[mtu-fragmentation]]

## Voir aussi

- [[Ports réseaux]] · [[Cession TCP]]
- [[Bastion]]