
La création d'une VM Azure se fait par l'ajout d'une ressource : 
![[Pasted image 20240229151133.png]]

![[Pasted image 20240229151300.png]]

Une VM doit avoir un **admin user** de défini (ce qui se fait dans l'onglet "Basics" de la création de la VM, en spécifiant un nom d'utilisateur et un mot de passe).
Cet utilisateur peut être utilisé pour se [[Connection à une VM Azure|connecter]] à la VM.

Une machine virtuelle doit faire partie d'un [[Virtual Private Network (VPN)|VPN]] Azure, et d'un [[Sous-réseau|sous-réseau]].
Une [[Adresse IP|adresse IP]] publique peut être également définie.
Une VM est associée à un [[Network security group (NSG) Azure|network security group]], qui agit comme un [[Firewall|firewall]] filtrant le trafic entrant et sortant de la VM.