---
tags: [azure, app-service, plan]
---

# Azure App Service Plan

Un plan App Service définit un **ensemble de ressources de calcul** nécessaires à l'exécution d'une application web. Une ou plusieurs applications peuvent être configurées pour s'exécuter sur les mêmes ressources informatiques (ou dans le même plan App Service).

## Éléments définis par le plan

Chaque plan App Service définit les éléments suivants :

- Système d'exploitation (Windows, Linux)
- Région (USA Ouest, USA Est, etc.)
- Nombre d'instances de machine virtuelle
- Taille des instances de machine virtuelle (petite, moyenne ou grande)
- [[niveaux-tarifaires-azure-app-service|Niveau tarifaire]] (Gratuit, Partagé, De base, Standard, Premium, PremiumV2, PremiumV3, Isolé, IsoléV2)

## Unité d'échelle

Ainsi, le plan App Service est l'**unité d'échelle** des applications App Service. Quand vous créez un plan App Service dans une région (par exemple, Europe Ouest), un ensemble de ressources de calcul est créé pour ce plan dans cette région. Toutes les applications que vous placez dans ce plan App Service s'exécutent sur ces ressources de calcul telles que définies par votre plan App Service.

Il est possible de **déplacer une application d'un plan App Service à un autre**, si on veut par exemple en **isoler** les possibilités de [[mise-a-lechelle-azure-app-service|mise à l'échelle]].

## Voir aussi

- [[niveaux-tarifaires-azure-app-service]]
- [[mise-a-lechelle-azure-app-service]]
- [[azure-app-services]]
