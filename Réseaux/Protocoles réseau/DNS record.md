
Un enregistrement [[Domain Name System (DNS)|DNS]] (Domain Name System record) est une instruction stockée dans une base de données DNS qui indique comment convertir des noms de domaine en [[Adresse IP|adresses IP]] (Internet Protocol) et vice versa.

### Types Principaux d'Enregistrements DNS

- **A (Address Record)** : Associe un **nom de domaine** à une adresse IP version 4 (**IPv4**).
- **AAAA (IPv6 Address Record)** : Associe un **nom de domaine** à une adresse IP version 6 (**IPv6**).
- **CNAME (Canonical Name Record)** : Associe un alias de domaine à un autre nom de domaine (le nom canonique). Ce type d'enregistrement est utilisé pour **associer des sous-domaines** à un domaine principal.
- **MX (Mail Exchange Record)** : Spécifie les serveurs de messagerie responsables de recevoir les emails pour le domaine.
- **NS (Name Server Record)** : Indique quels serveurs DNS sont autoritatifs pour le domaine. Cela permet de **déléguer la gestion des sous-domaines** à d'autres serveurs DNS.
- **TXT (Text Record)** : Permet aux administrateurs de domaine d'insérer du texte arbitraire dans le DNS, souvent utilisé pour des **vérifications de propriété de domaine** ou des **politiques de sécurité** comme SPF (Sender Policy Framework) et DKIM (DomainKeys Identified Mail).
- **SRV (Service Record)** : Fournit des informations sur les services disponibles dans le domaine, y compris le nom du service, le protocole, le port, et le poids pour l'équilibrage de charge.
