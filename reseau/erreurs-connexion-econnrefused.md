---
tags: [reseau, diagnostic, tcp, errno]
---

# Erreurs de connexion — lire un errno comme une coordonnée

Chaque erreur réseau désigne la **couche** où la chaîne s'est rompue. C'est ce qui permet de passer du symptôme à l'hypothèse sans chercher au hasard.

## Table de correspondance

| Erreur | Couche | Ce qui s'est passé | Où chercher |
|--------|--------|--------------------|-------------|
| `ENOTFOUND` / `NXDOMAIN` | 7 — DNS | Le nom n'a pas résolu, aucun paquet n'est parti | Enregistrement absent, mauvais résolveur, zone privée non jointe |
| `ECONNREFUSED` | 4 — TCP | L'hôte a répondu `RST` : rien n'écoute sur ce port | Service arrêté, mauvais port, bind sur `127.0.0.1` |
| `ETIMEDOUT` | 3 — IP | Aucune réponse, paquets jetés en silence | Firewall en `DROP`, NSG, route absente, hôte éteint |
| `ECONNRESET` | 4 — TCP | La connexion **existait** et a été coupée net | Crash du service, timeout proxy, keep-alive fermé en amont |
| `EHOSTUNREACH` | 3 — IP | Pas de route vers la destination | Table de routage, passerelle, VPN tombé |
| `EADDRINUSE` | 4 — TCP | Port déjà occupé localement | Process résiduel, ou [[ports-ephemeres-time-wait]] |
| `CERT_HAS_EXPIRED` | 6 — TLS | Connexion établie, **confiance** non | Voir [[certificat-x509]], [[chaine-de-certificats]] |
| `502` / `504` | 7 — HTTP | Tout a marché jusqu'au proxy, l'amont a lâché | Health probe backend, timeout proxy |

## La distinction qui règle la moitié des cas

- **`ECONNREFUSED`** = tu **as atteint la machine**. Réseau, routage et firewall ont tous fonctionné — c'est le **service** qui manque. Ne pas fouiller les règles réseau.
- **`ETIMEDOUT`** = tu n'as **jamais eu de réponse**. Le service n'est probablement même pas au courant de la tentative.

## Cause n°1 de ECONNREFUSED en conteneur

Le service écoute sur `127.0.0.1` au lieu de `0.0.0.0` : joignable depuis l'intérieur du conteneur, refusé depuis l'extérieur.

```bash
ss -tlnp | grep :8080
# 127.0.0.1:8080  -> KO depuis l'exterieur
# 0.0.0.0:8080    -> OK
```

Second suspect : depuis un conteneur, `localhost` désigne **le conteneur lui-même**, pas l'hôte ni le service voisin. Utiliser le nom du service (`db:5432`) ou `host.docker.internal`.

## Voir aussi

- [[diagnostic-par-couche]]
- [[Protocole TCP]]
- [[Ports réseaux]]
- [[Firewall]]
