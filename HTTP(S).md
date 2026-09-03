---
tags: [reseau, http, https, tls, osi]
---

# HTTP(S)

Protocole applicatif ([[Couche applicative du modèle OSI|couche 7]]) en **question-réponse**, **sans état**, historiquement en texte.

## Sans état — ce que ça implique

Le serveur ne se souvient de **rien** d'une requête à l'autre. Tout ce qui ressemble à une session — cookies, jetons — est un état que **le client renvoie à chaque fois**, par ses propres moyens.

## Le « S » de HTTPS

HTTPS n'est pas un autre protocole : c'est HTTP **dans un tunnel [[https-vue-ensemble|TLS]]**. Aucune ligne de HTTP ne change.

```
HTTP  →  TLS  →  TCP  →  IP
```

TLS apporte trois garanties distinctes :

| Garantie | Sens |
|----------|------|
| Confidentialité | Un tiers sur le chemin ne peut pas lire |
| Intégrité | Il ne peut pas modifier sans être détecté |
| Authentification | Le certificat prouve l'identité du serveur |

## Deux en-têtes de routage, deux couches

| | Rôle | Chiffré ? |
|---|---|---|
| **SNI** (extension TLS) | Quel certificat présenter | Non — en clair |
| **`Host`** (en-tête HTTP) | Quel site servir | Oui, dans le tunnel |

Leur redondance apparente s'explique par la couche : le SNI est lu **avant** que le tunnel n'existe. C'est aussi le seul élément qu'un firewall peut inspecter pour filtrer par domaine.

## Ports

`80` en clair, `443` en TLS — et `443/UDP` pour HTTP/3.

## Voir aussi

- [[Requêtes HTTP]] — méthodes, en-têtes, statuts
- [[http-versions]] — HTTP/1.1, 2, 3
- [[cout-rtt-connexion-https]] — le budget en allers-retours
- [[connexion-https-deroule]] — le déroulé complet
