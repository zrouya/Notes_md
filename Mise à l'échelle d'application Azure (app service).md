
Dans les niveaux [[Niveaux tarifaires Azure App Service#|Gratuit et Partagé]], une application reçoit des minutes processeur sur une instance de machine virtuelle partagée et ne peut pas être montée en charge.

Dans d’autres niveaux, une application s’exécute et est mise à l’échelle comme suit :

- Une application s’exécute sur **toutes les instances de machine virtuelle** configurées dans le plan App Service.
- Si **plusieurs applications** sont dans le même plan App Service, elles partagent toutes les mêmes instances de machine virtuelle.
- Si vous avez plusieurs **emplacements de déploiement** pour une application, tous les emplacements de déploiement s’exécutent également sur les mêmes instances de machine virtuelle.
- Si vous activez les journaux de diagnostic, effectuez des sauvegardes ou exécutez des tâches web, ils utilisent également des cycles d’UC et de la mémoire sur ces instances de machine virtuelle.