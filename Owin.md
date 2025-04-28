
Couche d'abstraction entre les application Web .Net et les serveurs HTTP hôtes.
Chronologiquement : 
1. **System.Web** = l’ancien pipeline ASP .NET, lourd et couplé à IIS.
2. **OWIN** = abstraction externe, plus légère, middleware-based, découplée de System.Web.
3. **ASP .NET Core** = réécriture complète sans System.Web, avec un pipeline middleware inspiré d’OWIN mais géré « en interne ».

https://learn.microsoft.com/fr-fr/aspnet/aspnet/overview/owin-and-katana/an-overview-of-project-katana