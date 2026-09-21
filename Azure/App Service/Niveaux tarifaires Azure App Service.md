
Le _niveau tarifaire_ d’un plan App Service détermine les fonctionnalités App Service associées et le prix correspondant. Il existe plusieurs catégories de niveaux tarifaires :

1. **Calcul partagé**
	Les deux niveaux de base, **Gratuit** et **Partagé**, exécutent une application sur la même machine virtuelle Azure que les autres applications App Service, y compris les applications d’autres clients.
	Ces niveaux allouent des quotas de processeur à chaque application, et **les ressources ne peuvent pas faire l’objet d’un [[Autoscaling Azure App Service|scale-out]]**.

2. **Calcul dédié**
	Les niveaux **De base**, **Standard**, **Premium**, **PremiumV2** et **PremiumV3** exécutent les applications sur des **machines virtuelles Azure dédiées**.
	Seules les applications qui se trouvent dans un même plan App Service partagent les mêmes ressources de calcul.
	Plus le niveau est élevé, plus vous disposez d’instances de machine virtuelle pour un scale-out.

3. **Isolé**
	Les niveaux **Isolé** et **IsoléV2** exécutent des **machines virtuelles Azure dédiées** sur des **réseaux virtuels Azure dédiés**.
	Il fournit à vos applications l’**isolement réseau** au-dessus de l’**isolation du calcul**.
	Il fournit les fonctionnalités de mises à l’échelle maximales.