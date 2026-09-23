---
tags: [azure, app-service, tarification]
---

# Niveaux tarifaires Azure App Service

Le _niveau tarifaire_ d'un plan App Service détermine les fonctionnalités App Service associées et le prix correspondant. Il existe plusieurs catégories de niveaux tarifaires.

## Calcul partagé

Les deux niveaux de base, **Gratuit** et **Partagé**, exécutent une application sur la même machine virtuelle Azure que les autres applications App Service, y compris les applications d'autres clients. Ces niveaux allouent des quotas de processeur à chaque application, et **les ressources ne peuvent pas faire l'objet d'un [[autoscaling-azure-app-service|scale-out]]**.

## Calcul dédié

Les niveaux **De base**, **Standard**, **Premium**, **PremiumV2** et **PremiumV3** exécutent les applications sur des **machines virtuelles Azure dédiées**. Seules les applications qui se trouvent dans un même plan App Service partagent les mêmes ressources de calcul. Plus le niveau est élevé, plus vous disposez d'instances de machine virtuelle pour un scale-out.

## Isolé

Les niveaux **Isolé** et **IsoléV2** exécutent des **machines virtuelles Azure dédiées** sur des **réseaux virtuels Azure dédiés**. Il fournit à vos applications l'**isolement réseau** au-dessus de l'**isolation du calcul**, et les fonctionnalités de mise à l'échelle maximales.

## Voir aussi

- [[azure-app-service-plan]]
- [[autoscaling-azure-app-service]]
- [[mise-a-lechelle-azure-app-service]]
