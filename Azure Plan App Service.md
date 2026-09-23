
Un plan App Service définit un **ensemble de ressources de calcul** nécessaires à l’exécution d’une application web.

Une ou plusieurs applications peuvent être configurées pour s’exécuter sur les mêmes ressources informatiques (ou dans le même plan App Service).

 Chaque plan App Service définit les éléments suivants :

- Système d’exploitation (Windows, Linux)
- Région (USA Ouest, USA Est, etc.)
- Nombre d’instances de machine virtuelle
- Taille des instances de machine virtuelle (petite, moyenne ou grande)
- [[Niveaux tarifaires Azure App Service|Niveau tarifaire]] (Gratuit, Partagé, De base, Standard, Premium, PremiumV2, PremiumV3, Isolé, IsoléV2)

Ainsi, le plan App Service est l’**unité d’échelle** des applications App Service.

Quand vous créez un plan App Service dans une région (par exemple, Europe Ouest), un ensemble de ressources de calcul est créé pour ce plan dans cette région.
Toutes les applications que vous placez dans ce plan App Service s’exécutent sur ces ressources de calcul telles que définies par votre plan App Service.

Il est possible de **déplacer une application d'un plan App Service à un autre**, si on veut par exemple en **isoler** les possibilités de [[Mise à l'échelle d'application Azure (app service)|mise à l'échelle]].